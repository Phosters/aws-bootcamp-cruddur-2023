# Week 2 — Distributed Tracing

### Distributed Tracing
Distributed tracing is a method of tracking application requests as they flow from frontend devices to backend services and databases. Developers can use distributed tracing to troubleshoot requests that exhibit high latency or errors.
With this our focus will be on Observability.
Observability is the extent to which you can understand the internal state or condition of a complex system based only on knowledge of its external outputs.
Observability has three pillars whuch helps in analyzing and tracing events in our application which are  Logs, metrics and traces.

This week focuses on the use of some tool we will use to perform observability in our application
These tools are :
- Honeycomb
- AWS X-ray
- AWS Cloud watch logs
- Roll bar

### Honeycomb Instrumentation in the Backend Flask


### AWS X-Ray Instrumentation 
Lets begin with installing AWS CLI and the framework for the frontend within the gitpod.yaml file:
These tasks are part of a configuration file for a tool called "Theia" that allows developers to run a cloud-based Integrated Development Environment (IDE). The tasks are defined using the YAML syntax and describe two separate jobs that will be executed sequentially.

The first job, named "aws-cli", sets up the AWS Command Line Interface (CLI) in the environment. It defines an environment variable called "AWS_CLI_AUTO_PROMPT" with a value of "on partial", which enables automatic prompting for certain CLI commands. It also includes an "init" section that contains shell commands to be run before the job executes. These commands do the following:

Change the current working directory to "/workspace".
Download the AWS CLI installation archive from the AWS website using the "curl" command.
Unzip the archive and install the AWS CLI using the "sudo" command.
Change the current working directory back to the root of the Theia workspace.
The second job, named "react-js", installs the React.js frontend framework in the environment. It contains a "command" section that specifies shell commands to be executed. These commands do the following:

Change the current working directory to the "frontend-react-js" directory.
Install the necessary Node.js packages using the "npm -install" command.
Overall, these tasks automate the setup of two important development tools - the AWS CLI and the React.js framework - so that they are readily available for use in the cloud-based IDE.
Do this with this code within the gitpod.yaml

```
tasks:
- name: aws-cli
  env: 
    AWS_CLI_AUTO_PROMPT: on partial 
    init: |
      cd /workspace
      curl -fSsl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
      unzip -qq awscliv2.zip
      sudo ./aws/install --update
      cd $THEIA_WORKSPACE_ROOT
- name: react-js
  command: |
   cd frontend-react-js
   npm -install
   
```

Locate the frontend and install npm with this ``` npm install ```

Now install the AWS SDK in the requirements.txt document with this : ``` aws-xray-sdk ```

Export the region you would want to work in, in my case I will use North Virginia with this:

```
export AWS_REGION="us-east-1"

```

In the context of Amazon Web Services (AWS), the AWS_REGION environment variable is used to specify the AWS region that an AWS client or SDK should connect to. The value "us-east-1" represents the US East (N. Virginia) region in AWS, which is one of the most commonly used regions in the platform, would do that with this:

```

gp env AWS_REGION="us-east-1"

```

