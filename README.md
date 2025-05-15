# volume-tags
Update Volume and Snapshot tags based on their parent volumes and EC2 instances

I found in my AWS account that I had untagged resources that were showing up in my Cost Explorer reports. And I wanted an easy way to ensure the EBS Volumes and Snapshots were tagged.

The script
- Confirms that all EC2 instances have the required tag, if not, it stops and the list of untagged EC2 is reported
- Finds all volumes without the tag and tags them based on their parent EC2 instance. If no parent instance is found, then the tag is set to "empty"
  - After tagging all the volumes, it double checks that the volumes actually got tagged and if not, exits with an error
- Finds all snapshots and tags them based on their parent EBS volume. If no parent volume is found, then the tag is set to "empty"
  - After tagging all the snapshots, it double checks that the snapshots actually got tagged and if not, exits with an error

I run this in CloudShell to avoid permission issues and you can direct the requests to another region if desired.

## Usage

```
usage: volume-tags.sh [ -h | --help ] [ -r | --region <region: us-east-2> ] <Case Sensisitive Tag to use>
```

## Usage Example

```
./volume-tags.sh -r eu-west-1 Project
Using --region eu-west-1
Using Project
Using AccountID: 111111111111
Looking for EC2 Instances missing the Tag: Project      Found 0 instances 
Looking for EBS Volumes missing the Tag: Project        Found 1 volumes
Attempting to set Volume Tags (Project) to correspond to their EC2 instances
Finding EC2-Instance for Volume (vol-01010101010101010)         i-0c001122334455667
Getting Project setting for i-0c001122334455667         EKS_Workshop
Setting Tag:Project to Valume:EKS_Workshop for vol-01010101010101010    Success
Looking for EBS Volumes missing the Tag: Project        Found 0 volumes
Looking for EBS Snapshots missing the Tag: Project      Found 0 Snapshots
```
