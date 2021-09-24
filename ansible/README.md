Create a compute engine template with the following system specifications:
    1. CentOS 7
    2. 8GB memory.
    3. 6 CPU cores
    4. SSD disk with 300GB.
    5. Create one server using the template using ansible.


## Steps:
1. Create GCP Account
2. Create GCP Service account (json key)
3. pip install requests google-auth
4. ansible-galaxy collection install google.cloud
5. ansible-playbook gcp_compute.yml
6. Validate with "gcloud compute instances describe vm-instance-1 --zone=us-west1-a"

## Findings
1. GCP has pre-determined machine types with set cpu and mem. There was no combination of 6CPU an 8GB memoery avaiable. The one I deployed (n1-standard-1) is 1cpu and 4GB ram. There is a possibility for customization, but I do not believe its supported in google.cloud plugin for Ansible.
2. Disk type is determined by machine type. N1 VMs come standard with SSD (https://cloud.google.com/compute/docs/general-purpose-machines)
3. json key for GCP service account is not checked into git. To replicate, provide your own JSON key.


## Console Output
- cpuPlatform: Intel Broadwell
- creationTimestamp: '2021-09-23T23:10:16.972-07:00'
- deletionProtection: false
- disks:
    - autoDelete: true
    - boot: true
    - deviceName: persistent-disk-0
    - diskSizeGb: '300'
    - guestOsFeatures:
        - type: UEFI_COMPATIBLE
    - index: 0
    - interface: SCSI
    - kind: compute#attachedDisk
    - licenses:
        - https://www.googleapis.com/compute/v1/projects/centos-cloud/global/licenses/centos-7
    - mode: READ_WRITE
    - source: https://www.googleapis.com/compute/v1/projects/pacific-byte-327001/zones/us-west1-a/disks/vm-instance-1
    - type: PERSISTENT
- fingerprint: jGcHemYvnbQ=
- id: '7796763854882526503'
- kind: compute#instance
- labelFingerprint: gHFoMnyTv7s=
- labels:
    - environment: testing
- lastStartTimestamp: '2021-09-23T23:10:24.728-07:00'
- machineType: https://www.googleapis.com/compute/v1/projects/pacific-byte-327001/zones/us-west1-a/machineTypes/n1-standard-1
- metadata:
    - fingerprint: AFNZGx7HlJw=
    - kind: compute#metadata
- name: vm-instance-1
- networkInterfaces:
    - accessConfigs:
        - kind: compute#accessConfig
        - name: External NAT
        - natIP: 34.83.85.239
        - networkTier: PREMIUM
        - type: ONE_TO_ONE_NAT
    - fingerprint: PjJ_2MLfshQ=
    - kind: compute#networkInterface
    - name: nic0
    - network: https://www.googleapis.com/compute/v1/projects/pacific-byte-327001/global/networks/network-instance
    - networkIP: 10.138.0.2
    - stackType: IPV4_ONLY
    - subnetwork: https://www.googleapis.com/compute/v1/projects/pacific-byte-327001/regions/us-west1/subnetworks/network-instance
- scheduling:
    - automaticRestart: true
    - onHostMaintenance: MIGRATE
    - preemptible: false
- selfLink: https://www.googleapis.com/compute/v1/projects/pacific-byte-327001/zones/us-west1-a/instances/vm-instance-1
- shieldedInstanceConfig:
    - enableIntegrityMonitoring: true
    - enableSecureBoot: false
    - enableVtpm: true
- shieldedInstanceIntegrityPolicy:
    - updateAutoLearnPolicy: true
- startRestricted: false
- status: RUNNING
- tags:
    - fingerprint: 42WmSpB8rSM=
- zone: https://www.googleapis.com/compute/v1/projects/pacific-byte-327001/zones/us-west1-a