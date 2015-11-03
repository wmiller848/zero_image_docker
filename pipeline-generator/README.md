# Deploying the Pipeline #
This is generally a one time process to get the jobs on a  given Jenkins server

## Dry Run ##
Its first recommended to do a 'dry run' to insure that the pipleine will construct
itself correctly

Run the following commands from the directory this README is located in.

`bundle install -j4`
`generate pipeline -d bootstrap pipeline/`

## Uploading to Jenkins ##

Once you have confirmed the pipeline builds correctly with a 'dry run' we are ready
to upload it the remote Jenkins server.

First we need edit the 'login.yml' file to reflect three things; server ip, username, password.
The username and password can be you SSO login.

** IMPORTANT - NEVER CHECK IN OR SHARE LOGIN.YML **

Once you have updated the file with the correct information we can push our jobs
to the remote server

Run the following command

`generate pipeline -c login.yml bootstrap pipeline/`

## Wrapping Up ##
Once the pipeline is on the remote Jenkins, it will automatically be rebuilt every
time there is a commit to the main branch. Thus if you need to make a manual change
or rerun the yo ctct-cd command, simply run the 'dry run' to make sure the pipeline
builds and simply checking and merge the change into the main branch, the changes
will automatically be picked up.
