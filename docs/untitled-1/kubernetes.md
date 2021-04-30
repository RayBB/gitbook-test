# Kubernetes

A proof-of-concept has been developed for Kubernetes-based CI which will, in the future, allow trusted developers to be able to dynamically \(on demand\) spin up their own “dev.openlibrary.org”-like review app pod for their branch, connected with OpenLibrary production keys / db / solr / lending.

This will enable trusted contributors to:

* develop and test against live data
* not have to ask for branch switching on the current single dev server, dev.openlibrary.org
* post review app links \(to pods\) in PRs

This repo is used as a building block: [https://gitlab.com/internetarchive/kre8](https://gitlab.com/internetarchive/kre8)

## Plans for Kubernetes \(2019-08\)

1. no near-term plans \(def not before October\) yet to move production piece of OL into kubernetes
2. there is a git.archive.org gitlab repo containing the altered version of OL docker \(internal access only — we’ll grant spin-up-only access to these pods to blessed/trusted members — no ssh read / writes of the pods themselves\)
3. near-term plan is to fix dev.openlibrary.org to be a test for `master` only \(before openlibrary.org gets deploys\)
4. kube pods will be used to allow trusted members to spin up versions of OL to test specific branches on/in prod-like environments
5. down the road, kubernetes may help us scale \(as you suggest\) memcached, solr, and perhaps our gunicorn web heads
6. there is a well written post here on [setting up dynamic nfs server in kubernetes](https://redblink.com/setup-nfs-server-provisioner-kubernetes/)

