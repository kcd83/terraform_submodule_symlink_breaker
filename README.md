# Demo of Terraform sub-module bug inside Docker for Windows mounts

Reproduce the issue with:

    docker run -i -t -v ${pwd}:/mount -w /mount/workspace --entrypoint sh hashicorp/terraform:light
    terraform init
    terraform apply


The error is:

    data.local_file.hello: Refreshing state...
    data.local_file.hello: Refreshing state...

    Error: Error refreshing state: 1 error(s) occurred:

    * module.one_hello.data.local_file.hello: 1 error(s) occurred:

    * module.one_hello.data.local_file.hello: data.local_file.hello: open /mount/workspace/.terraform/modules/3a1ec902e54a850aaaf6b8f2a65a9bd5/hello.txt: no such file or directory


However, we can verify that outside of the Windows mount everything is fine:

    docker run -i -t --entrypoint sh hashicorp/terraform:light
    git clone https://github.com/kcd83/terraform_submodule_symlink_breaker.git
    cd terraform_submodule_symlink_breaker/workspace
    terraform init
    terraform apply

You will get:

    data.local_file.hello: Refreshing state...
    data.local_file.hello: Refreshing state...

    Apply complete! Resources: 0 added, 0 changed, 0 destroyed.
