This Image is built from the
parent Image: https://github.com/CircleCI-Public/cimg-ruby/blob/main/3.1/browsers/Dockerfile
Which is descendant of this one:
https://github.com/CircleCI-Public/cimg-ruby/blob/main/3.1/node/Dockerfile
Which is a child of this:
https://github.com/CircleCI-Public/cimg-ruby/blob/main/3.1/Dockerfile

The last one includes Ruby 3.1.3 version

To build the Image just execute this command in the folder that contains Dockerfile

cd to version folder 3.1.3 and run
docker image build -t atkinsandpearce/ruby:3.1.3-browsers-mssql .
And than publish to Dockerhub
docker push atkinsandpearce/ruby:3.1.3-browsers-mssql

atkinsandpearce - Account Name
ruby            - Repository Name
3.1.3-browsers-mssql - Tag Version

Include appropriate image name in circleci/config.yml
like - image: atkinsandpearce/ruby:3.1.3-browsers-mssql

