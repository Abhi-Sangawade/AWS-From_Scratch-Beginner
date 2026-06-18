# Day 13 — Building an AMI from My EC2 Instance

## What I'm Doing Today

Today's task is all about turning a running EC2 instance into a reusable image. I have an instance called `nautilus-ec2` running in `us-east-1`, and my goal is to bake it into an AMI named `nautilus-ec2-ami`. Once that's done, I need to confirm the AMI actually finishes processing and lands in the `available` state — that's the real proof the job is done, not just that I clicked the right buttons.

## Why This Matters

An AMI is basically a frozen snapshot of an instance — its OS, installed packages, configs, and attached volumes — packaged up so I (or anyone on my team) can spin up identical servers on demand. Instead of manually rebuilding a server from scratch every time, I just launch from the AMI and I'm done in minutes. This is the backbone of how teams handle auto-scaling, disaster recovery, and consistent environments across dev/stage/prod.

## Before I Start (Prerequisites)

A few things I made sure of before touching the console or CLI:

- My IAM user/role has `ec2:CreateImage`, `ec2:DescribeImages`, and `ec2:DescribeInstances` permissions — without these the create-image call just fails silently or throws an auth error.
- The AWS CLI is configured with the right credentials and default region (`aws configure`), or I explicitly pass `--region us-east-1` on every command.
- I know roughly how much data is on the instance's volumes, since larger volumes mean longer AMI creation times.

## Doing It Through the Console

1. Log into the AWS Console and switch the region selector to **us-east-1** (easy step to forget, and it'll waste your time if you skip it).
2. Open the **EC2 Dashboard** and click into **Instances**.
3. Find and select `nautilus-ec2` from the list.
4. From the top menu: **Actions → Image and templates → Create Image**.
5. Type in the image name exactly as required: `nautilus-ec2-ami`.
6. Leave the rest of the settings on default (no need to fiddle with volume settings unless told otherwise).
7. Hit **Create Image** and let AWS do its thing.
8. Head to the **AMIs** section in the left sidebar and keep an eye on the **Status** column until it flips to `available`.

## Doing It Through the CLI (My Preferred Way)

I like the CLI better since it's scriptable and gives me cleaner confirmation of what actually happened.

**Step 1 — Find the instance ID:**

```bash
aws ec2 describe-instances \
  --filters "Name=tag:Name,Values=nautilus-ec2" \
  --query "Reservations[].Instances[].InstanceId" \
  --output text \
  --region us-east-1
```

**Step 2 — Kick off the image creation:**

```bash
aws ec2 create-image \
  --instance-id <instance-id> \
  --name "nautilus-ec2-ami" \
  --no-reboot \
  --region us-east-1
```

This returns an `ImageId` right away — note it down, I'll need it for checks.

**Step 3 — Poll the status until it's ready:**

```bash
aws ec2 describe-images \
  --owners self \
  --query "Images[*].[ImageId,Name,State]" \
  --output table \
  --region us-east-1
```

**Bonus — let the CLI wait for me instead of me refreshing manually:**

```bash
aws ec2 wait image-available \
  --image-ids <ami-id> \
  --region us-east-1
```

This command just blocks until the AMI is `available`, which is honestly more convenient than running `describe-images` in a loop.

## A Couple of Things I'd Add That Aren't in the Original Steps

- **Tag the AMI** right after creation so it's easy to find later and tie back to a cost center or project:
  ```bash
  aws ec2 create-tags --resources <ami-id> --tags Key=Project,Value=Nautilus --region us-east-1
  ```
- **Check the associated snapshot too.** Every AMI is backed by an EBS snapshot — if I ever need to clean up storage costs, I have to delete both the AMI (deregister) and its snapshot, not just one of them.
- **Consider copying the AMI to another region** if disaster recovery across regions is part of the bigger picture — `aws ec2 copy-image` handles that.
- **Clean up unused AMIs periodically.** They're cheap individually, but old AMIs and their snapshots pile up storage charges over time if nobody deregisters them.

## --no-reboot: What It Actually Means

Using `--no-reboot` tells AWS not to restart the instance before snapshotting it. It's faster and avoids downtime, but there's a small tradeoff — if there's data still being written to disk at the exact moment of the snapshot, the image could end up slightly inconsistent. For a quick lab task like this it's fine; for production-critical systems, I'd lean toward letting it reboot for a cleaner, consistent image.

## Roadblocks I Ran Into

- Had to double check the instance tag matched exactly (`Name=nautilus-ec2`) — a typo here returns an empty result and wastes time debugging the wrong thing.
- Waiting for the state to transition from `pending` to `available` took a few minutes — patience over panic.
- Initially mixed up "image" terminology with "snapshot" — worth remembering an AMI and an EBS snapshot are related but not the same resource.

## What I Walked Away Understanding

- An AMI is a portable, launchable template — not just a backup file sitting somewhere.
- The CLI workflow (`create-image` → `describe-images` / `wait image-available`) is more reliable for automation than clicking through the console every time.
- `--no-reboot` is a speed-vs-consistency tradeoff worth understanding rather than blindly using.

## Interview Angle

**Q: Why would you use an AMI instead of just manually configuring a new server each time?**

A: Because it guarantees consistency — every instance launched from the same AMI starts identical, with the same OS, packages, and configuration. That consistency is what makes auto-scaling, fast disaster recovery, and repeatable deployments possible without manual setup every time.
