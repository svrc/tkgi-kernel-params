# Tanzu Kubernetes Grid Integrated (TKGI) / PKS Kernel Params

## What does this do?

This will ensure that TKGI/PKS clusters can have custom kernel parameters set.  These should survive all upgrades/updates and be applied to all clusters.  

Edit `addon.yml` properties to customize your desired kernel parameters.

If you want to target specific clusters, you can edit the addon YAML with [inclusion or exclusion rules](https://bosh.io/docs/runtime-config/#placement-rules)


## How do I install it?

1. Open a shell prompt on a BOSH CLI with access to your TKGI bosh director, such as Ops Manager.
2. Export your BOSH credentials to the enviornment.  These can be accessed via the Ops Manager GUI -> BOSH Director Tile -> Credentials Tab -> Bosh Commandline Credentials.

e.g.
```
export BOSH_CLIENT=ops_manager BOSH_CLIENT_SECRET=fakesecret BOSH_CA_CERT=/var/tempest/workspaces/default/root_ca_certificate  BOSH_ENVIRONMENT=10.0.0.10
```
3. Copy or clone this repository onto this BOSH CLI workstation and create+upload the BOSH release to the director

```
git clone https://github.com/svrc/tkgi-kernel-params && cd tkgi-kernel-params
git submodule init ; git submodule update
cd os-conf-release
bosh create-release --force
bosh upload-release ./dev_releases/os-conf/os-conf-21.0.0+dev.1.yml

```
4. Configure the addon from this repo
```
cd ..
bosh -n update-config --name=tkgi-kernel-params --type=runtime ./addon.yml
```
5. Update your TKGI clusters via the PKS CLI "upgrade/update-cluster" and/or Ops Manager "Apply Pending Changes" button with the TKGI upgrade errand enabled.  This addon will automatically be installed on all worker nodes with the default manifest `addon.yml`

