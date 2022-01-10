### Learning to use [AWS copilot](https://aws.amazon.com/containers/copilot/)
1. deploy a simple web page from an nginx container
1. set up https
1. configure simple CI/CD pipeline with test and prod environment


##Steps

####1)  installed AWS copilot on local machine: 

`brew install aws/tap/copilot-cli`

####2) wrote the simple app

 - aws-copilot-pipeline
   - page
     - Dockerfile
	 - index.html
	 - img
	   - rick.png
   
**_important_**: all copilot CLI commands are run __from the root git__ directory

####3) tested locally

####4) deployed to AWS

```
copilot init --app page \
  --name web \
  --type "Load Balanced Web Service" \
  --dockerfile "page/Dockerfile" \
  --deploy
```

####5) tested deleting everything

```
copilot app delete
```

####6) set it up for HTTPS (hosted zone already set up in route53)

```
copilot app init page --domain dazl.ca
```

####7) re-deployed

```
copilot init --app page \
  --name web \
  --type "Load Balanced Web Service" \
  --dockerfile "page/Dockerfile" \
  --deploy
```

As a result the test environment page [is available](https://web.test.page.dazl.ca)

####8)add a production environment
Some interaction is required to re-use existing VPC and subnets

```
copilot env init --name prod --profile <my-aws-profile-name> --prod
```

**_note_**: this lets you use a different region and aws account if desired and later add a pipeline that spans across regions -- I'm impressed

####8) set up a CI/CD pipeline

 - push to github

```
copilot pipeline init \
--url https://github.com/dazl/aws-copilot-pipeline.git \
--environments "test,prod" 
```

 - follow the instructions returned by the CLI and the prod environment [appears](https://web.prod.page.dazl.ca). 
 - try a code change and see the build automatically start and being pushed to test and then prod. See CodeBuild and CodePipeline in the AWS console 
 
 
