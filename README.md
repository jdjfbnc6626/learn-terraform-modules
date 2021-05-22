# Reference  https://learn.hashicorp.com/tutorials/terraform/module-create?in=terraform/modules
  505  mkdir learn-terraform-modules
  506  cd learn-terraform-modules/
  507  touch main.tf
  508  touch variables.tf
  509  touch outputs.tf
  510  terraform init
  511  terraform validate
  
  Apply complete! Resources: 22 added, 0 changed, 0 destroyed.
  Destroy complete! Resources: 22 destroyed.
  
JJUser:~/environment/learn-terraform-modules $ ls -a .terraform/modules/
.  ..  ec2_instances  modules.json  vpc

Apply complete! Resources: 23 added, 0 changed, 0 destroyed.

# Modules creation steps
  520  mkdir -p modules/aws-s3-static-website-bucket
  521  touch LICENSE
  522  cd modules/aws-s3-static-website-bucket/
  523  touch main.tf && touch variables.tf && touch outputs.tf
  
# This was a screw up. Did not back out to correct directory, so Terraform initiated modules vs main in wrong spot. Creates conflict
  524  cd ..
  525  terraform get
  526  terraform validate
  527  terraform apply
# Had to delete a dependency folder, then re-init whole project
  531  terraform init
  532  terraform get
  533  terraform validate
  538  terraform apply
  
# Nice CLI one-liners to populate web server
  539  mkdir modules/aws-s3-static-website-bucket/www
  540  cd modules/aws-s3-static-website-bucket/www
  541  touch index.html && touch error.html
  542  cd ../../../
  543  aws s3 cp modules/aws-s3-static-website-bucket/www/ s3://$(terraform output -raw website_bucket_name)/ --recursive
  
https://terraform-jj-modules-demo.s3-us-west-2.amazonaws.com/index.html

# Clean up
  545  aws s3 rm s3://$(terraform output -raw website_bucket_name)/ --recursive
  546  terraform destroy

# Created a .gitignore
- So will need a 'terraform init' to start project