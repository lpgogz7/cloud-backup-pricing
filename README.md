# cloud backup and restore: real pricing broken down, which plan actually costs less, and what to set up before you ever need a recovery

When data goes wrong, only one question matters: can you restore? But whether the answer is yes gets decided long before that moment — where you back up, how much you pay, and whether the restore path has ever actually been tested. This article walks through how cloud backup and restore works, then uses a verified real-world price sheet to show where the total-cost traps hide.

For readers in a hurry:

> **Most small-to-mid workloads** (one to a few servers, around 200GB of data): Sharktech's Acronis Cyber Protect managed backup is the low-friction starting point — **200GB base tier for $4/month**, covering physical servers, VMs, and cloud workloads on Windows, Linux, and macOS, with restore granularity from a single file to a full machine. On total cost over a year, **the annual plan ($24/year) wins for any workload above 200GB**; the monthly plan wins on flexibility. There's also a cheaper-per-TB DIY route via S3-compatible object storage at **$4.90/TB/month** if you bring your own backup software.
> 👉 [Check the configured price of the 200GB Acronis backup plan](https://portal.sharktech.net/cart.php?a=add&pid=648&configoption%5B1862%5D=200&configoption%5B1863%5D=0&billingcycle=monthly&aff=1626)

## What cloud backup and restore actually means

Cloud backup sends copies of your data to cloud servers over a secure connection; restore is the reverse operation — pulling files, folders, or an entire machine back from those copies when production data is lost or corrupted.

In practice there are three configuration directions: backing up on-premises data to the cloud, backing up cloud data to local servers, and cloud-to-cloud backup. Once the backup software and storage target are configured, most solutions let you set schedules, manage backup files, and throttle bandwidth, then run continuously. On the restore side, granularity varies: recovering one deleted file is a different operation from rebuilding a server from bare metal, and a good plan needs both [S2].

The typical use cases: data protection against accidental deletion and hardware failure, disaster recovery and business continuity against ransomware and outages, and archiving for compliance. Many organizations use cloud backup as the off-site third copy in a **3-2-1 strategy** — three copies of data, on two different media, one of them off-site [S2][S4].

People searching this term are usually trying to answer three questions: how do I back up, where do I put the copies, and will restore actually work when it counts. The rest of this article follows that order.

## Cloud backup vs. cloud storage: the distinction that decides whether restore can save you

These two get confused constantly, and the difference determines whether a deleted file is recoverable.

- **Cloud storage** means putting files on remote servers for access. Cloud files live independently of your production environment, so if you delete something locally, the cloud copy survives. That's the extent of its protection.
- **Cloud backup** means using dedicated backup software to send data to cloud storage, with full and incremental backups, where cloud versions track changes in the original data over time [S2].

There's a harsher truth underneath, and it comes from basic cloud operations: **replication is not backup**. When files get deleted or corrupted, sync and replication mechanisms faithfully propagate the damage to every copy. Cloud providers, in most cases, are not responsible for backing up your data — backups with independent retention and versions have to be implemented by the customer. Textbook material puts it bluntly, and it's worth internalizing [S4].

So "my data is in the cloud, I'm fine" misses the restore half of cloud backup and restore entirely. Cloud sync gives you durability — your data survives hardware failure. It does nothing against a slip of the hand, ransomware encrypting everything, or an application writing bad data. Those scenarios need backup copies with their own retention windows.

Snapshots are part of this picture too: they're point-in-time copies (usually incremental), and managed databases typically offer automated backups with point-in-time recovery windows — the ability to restore to any moment within the last 7 or 35 days, for example. Knowing what retention window your services actually provide is part of restore planning [S4].

## A real price sheet, torn down: four billing cycles and the overage trap

The clearest way to see how backup pricing works is a live example. Sharktech's Acronis Cyber Protect managed backup starts at $4/month on the official product page, with the final recurring price depending on the capacity and protection options you select in checkout [S12]. The base tier includes 200GB, and there are four billing cycles:

| Plan | Storage | Price | Overage | Best for | Get it |
| --- | --- | --- | --- | --- | --- |
| Acronis – Monthly | 200GB | $4.00/mo | $0.02/GB | Fluctuating or short-term workloads | [Order monthly](https://portal.sharktech.net/cart.php?a=add&pid=648&configoption%5B1862%5D=200&configoption%5B1863%5D=0&billingcycle=monthly&aff=1626) |
| Acronis – Quarterly | 200GB | $8.00 / 3 mo | $0.04/GB | Rarely the right choice (see below) | [Order quarterly](https://portal.sharktech.net/cart.php?a=add&pid=648&configoption%5B1862%5D=200&configoption%5B1863%5D=0&billingcycle=quarterly&aff=1626) |
| Acronis – Semi-Annual | 200GB | $12.00 / 6 mo | $0.06/GB | Same — beats nothing | [Order semi-annually](https://portal.sharktech.net/cart.php?a=add&pid=648&configoption%5B1862%5D=200&configoption%5B1863%5D=0&billingcycle=semiannually&aff=1626) |
| Acronis – Annual | 200GB | $24.00 / yr | $0.12/GB | Lowest total cost for steady workloads | [Order annually](https://portal.sharktech.net/cart.php?a=add&pid=648&configoption%5B1862%5D=200&configoption%5B1863%5D=0&billingcycle=annually&aff=1626) |
| S3 Object Storage | 1TB | $4.90/mo | +$0.90/TB egress beyond 1TB | DIY backup with your own software | [Order 1TB storage](https://portal.sharktech.net/cart.php?a=add&pid=643&carttpl=s3_storage_cart&billingcycle=monthly&configoption%5B1858%5D=13673&configoption%5B1859%5D=1&aff=1626) |

Now the structure worth understanding before you pick. With most cloud services, longer commitments make everything cheaper. Here, longer cycles lower the base-tier monthly equivalent but **raise the overage rate**: the annual plan's overage is six times the monthly plan's ($0.12/GB vs. $0.02/GB). The annual plan isn't uniformly cheaper — it's cheaper on the base storage and more expensive on every gigabyte past 200.

Run the actual math for data volumes above the base tier (X GB total, so X−200 GB of overage):

- **Yearly cost on monthly billing** = $48 + $0.24 × (X−200)
- **Yearly cost on annual billing** = $24 + $0.12 × (X−200)

The annual plan comes out ahead for every X above 200GB — at 250GB, that's $60/year vs. $30/year; at 2TB (2,048GB), roughly $492/year vs. $246/year. So for any steady workload you're confident you'll keep for a year, **annual is the cheapest total-cost option**.

The monthly plan earns its keep in three situations: your data volume swings unpredictably, the project has a defined end date, or you want to run a small trial before committing — because paying $4 month-to-month beats locking in $24 when you might stop at month three. The quarterly and semi-annual tiers are the awkward middle children: higher monthly equivalent than annual, higher overage than monthly. They lose on both axes, and there's no workload profile where they're the right answer.

One more comparison point to calibrate expectations. Third-party-compiled figures put Liquid Web's managed Acronis backups at $11/month for 100GB plus a base workload fee — reportedly reaching about $134/month at 1TB — and Acronis direct pricing at roughly $85/year per device without any storage included. Sharktech's $4/month for 200GB, billed by storage rather than by workload, sits well below both. The trade-off: Liquid Web bundles more hands-on managed support around backups, while this is a self-service add-on [S22][S14].

👉 [Start with the 200GB plan on monthly billing and see your real configured price](https://portal.sharktech.net/cart.php?a=add&pid=648&configoption%5B1862%5D=200&configoption%5B1863%5D=0&billingcycle=monthly&aff=1626)

Two optional add-ons affect the final bill. **File Sync & Share** — a Dropbox-style layer that can turn the backup target into a shareable drive — is priced separately ($0.03/GB/month at overage on the monthly cycle); MSPs tend to enable it, solo operators mostly skip it. The official product page says it plainly: check the itemized selection before confirming your order [S12][S22][S14].

## The cheaper-per-TB route: S3-compatible object storage for DIY backup

If you already run your own backup software — Restic, Borg, Duplicacy, Veeam, rclone — you don't need managed backup at all. You need storage, and Sharktech's second product line is S3-compatible object storage at a flat **$4.90/TB/month**, with 1TB of egress included and no API request fees, no tiered pricing, and no long-term contract. The same per-TB rate applies from 1TB to 100TB; egress beyond the included 1TB runs $0.90/TB [S26][S15].

For calibration, Backblaze B2 runs around $6.95/TB/month and Wasabi sits at a similar level, so $4.90/TB with bundled bandwidth lands at the aggressive end of the market [S11][S15].

Features relevant to backup specifically: object versioning, **Object Locking with retention policies** (the immutability layer that protects backups from ransomware and malicious deletion), and lifecycle rules for expiring old recovery points. It works with rclone, Cyberduck, Nextcloud, and S3 Browser out of the box [S26][S4].

Official positioning matters here too: this is capacity-first storage built on large HDD tiers with NVMe caching for active data. It's explicitly the wrong tool if you need a bootable volume, filesystem semantics, or stable low-latency block storage — for those, use a different product [S26].

👉 [Configure the 1TB S3 storage plan for $4.90/month](https://portal.sharktech.net/cart.php?a=add&pid=643&carttpl=s3_storage_cart&billingcycle=monthly&configoption%5B1858%5D=13673&configoption%5B1859%5D=1&aff=1626)

## How restore actually happens — and the drill you should run before disaster

Restore is a pre-incident activity, and Sharktech's own product page says as much: "make testing recovery part of the operating plan." The mechanics, on the Acronis side: you pick a recovery point and a target in the web console, then restore a single file, multiple folders, or a complete supported machine. Bare-metal recovery works even onto different hardware, without installing an OS first. The console runs independently of your server control panel, registers multiple systems, and shows backup status, activity, and storage consumption — which is how you catch failed jobs before the emergency, rather than during it [S12][S18][S22][S14].

Ransomware-specific restore details are worth knowing:

- **Scan before you restore**: Acronis's active anti-ransomware functionality scans backups for malware before recovery, so you don't reinfect a clean machine from an infected backup [S22].
- **AI-driven Active Protection** monitors processes continuously and blocks suspicious encryption behavior on the production side [S14].
- Newer Acronis versions add a **forensic backup** option that captures system state for post-incident investigation [S14].

Restore time depends on data size and the chosen recovery method — no vendor publishes hard numbers here, so when you plan your RTO (recovery time objective), leave margin based on your actual data volume. The standard drill: restore a backup to a staging machine or scratch environment, verify the data is valid and the process completes end to end, then repeat periodically. A backup that has never been restored is a hypothesis, not a plan [S4][S12].

## Who this fits, and who should spend their money elsewhere

The fit profile is fairly specific:

- **You already run, or are about to rent, a Sharktech dedicated server or VPS.** Backup and server at the same provider means no egress fees between your box and the backup target, and one support number for both. Sharktech has been around since 2003 and runs its own data centers in Los Angeles, Chicago, Denver, and Amsterdam [S22][S11].
- **Compliance or client trust requires the words "Acronis Cyber Protect" on the invoice.** You're buying the real platform, per-GB, rather than Acronis direct's per-workload licensing [S14][S22].
- **Your backup budget sits between "tape drive" and "enterprise Veeam."** The $4/month entry exists for small-workload operators who still want ransomware scanning on the restore side. Third-party comparisons note that Acronis differentiates from Veeam by bundling its own anti-malware layer, where Veeam relies on third-party threat detection [S22][S14].
- **MSPs and small IT shops** who resell backup: per-GB billing is easy to quote to clients, and the console supports managing many systems [S14][S22].

Just as clearly, several readers should stop here:

- **Individual users who just want files available in the cloud.** That's cloud storage, and paying monthly for managed backup is wasted money on this requirement.
- **Teams needing vendor-managed backup operations.** This is a self-service add-on; the 24/7 support is reactive, with an in-house engineering team at each data center — it doesn't include someone designing your retention strategy [S12][S22].
- **Readers who need a refund guarantee.** Third-party reviews show a Trustpilot rating around 3.5 on a small number of reviews, with praise concentrated on low-latency connectivity and responsive support, and one consistent complaint: no money-back guarantee. Treat any prepaid cycle as locked in [S22].
- **Multicloud estates.** If your workloads span AWS, Azure, and Google Cloud and you want one console with unified policies and cross-cloud copies, that's the problem Veeam's platform is built for. Sharktech's lineup is about backing up concrete machines and storing data cheaply; multicloud orchestration isn't its territory [S3].
- **Hyperscale deployments** — petabytes, custom retention across regions. Third-party guidance points those toward the big cloud providers or enterprise backup vendors [S14].
