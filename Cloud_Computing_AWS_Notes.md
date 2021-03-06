#Cloud Computing & AWS

## Defining Cloud Computing

- HAve computational resources or services available 
- THis can be either on the internet, or on an organisation-internal channel. A hybrid of hte two is also possible.
    - An example of this is banking, where confidential data will be managed on an internal cloud, but publically accessible files will be served from a public cloud
- Clouds are used by almost any larger organisation that requires computing services.

## AWS

- Amongst others, one of the biggest providers of cloud services
- used by a lot of companies
- Biggest advantage is that there is no longer a huge upfront cost to obtain machines 
- Also highly scalable with demand (can rent more or less computing capacity as needed)

## AWS Global Infrastructure

- AWS orders their services by Regions, each with mutiple availability zones
- Each region has at least 2 data centers to ensure availability
- Choosing a data center depends on a variety of factors:
    - Latency to data center (from user)
    - Cost of a particular region
    - legal considerations (some countries require data collected there to be handled there)
- Not all services are available in all regions
- Full list of regions and availability zones [here](https://aws.amazon.com/about-aws/global-infrastructure/)

## Common Use Cases

- IaaS(infrastructure as a service): The cloud provider offers only the infrastructure, and it is up to the cloud user to provide the OS, software and configuration
- PaaS (Platform as a Service): The cloud provider offers a platform (computign capacity as well as the operating system). The user must provide software, and configure the machine for purpose
- Saas (Software as a Service): The cloud provider provides a software package, which the user simply has to configure for their purpose.

## AWS Services

- Elastic Compute Cloud `EC2`
- Simple Storage Service `S3`
- Virtual Private Cloud (aka VBirtual Private Network) `VPC`
- Internet Gateway `IG`
- Route Tables `RT`
- Subnets `sn`
- Network Access Control `NALs`
- Security Groups `SG`
- Cloudwatch `CW`
- Simple Notification Service `SNS`
- Load Balancers `LB` - `ALB` - `ELB` - `NLB`
- Autoscaling Groups `ASG`
- Amazon Machine Image `AMI`
- Simple Queue Service `SQS`

Again, note that these are not always available in every region.

## Launching an Instance

- Requires a key. This can eithe rbe created or obtained from an administrator, depending on the organisation's setup.
- The key needs to be moved to a connecting machine's .ssh folder

## Exercise: Setup the node.js app to run in the cloud 
- Important to setup security group to control access. For example, here are the rules that shyoudl govern an instance running the previously contructed app:

```
1. port 22 to be connected to only from admin's IP
2. port 80 accessible to the world (this allows users to access the webpage via http)
3. port 3000 accessible (this is where the app will be listening)
```
- note that it is a good idea to limit the access to the instance as much as possible
- Make sure to follow org's naming convention for both security groups and app names
- if in doubt, AWS does provide guidance during the launching process.

### Provisioning the EC2 instances

### App Instance
These are essentially the same steps as before:
- Update and Upgrade
- install nginx
- nginx enabled
- check public IP globally

- install correct version of node & required dependencies
- upload app files using the `scp` command
- npm install
- npm start

The exact commands are the same as those in the previous `provision_app` sript, and it is possible to copy these across with scp.

**Important note: The following variables have to be updated**

- The `DB_HOSTS` variable (should become mongodb://[DB_INSTANCE_IP]:27017/posts)
- The `server` address in nginx's modified default file (should match the IP of the app instance)

### DB Instance
Now that the app machine is provisioned, need to set up the database instance.

Requirements are:

```
- allows port 22 access from admin machine
- allows access to mongodb port for the app instance

- mongodb installed
- has adjusted mongod.conf
```

The easiest way to set this up is at the security group stage. The steps are:

- First inbound rule: allow port 22 from your (the admin's) IP
- Second inbound rule: Allow port 27017 from the `app` instance

- After launching the instance, simply run the installation commands as they were in the `provision_db` script.



- Once this is done, we can set the environment variable `DB_HOST` on the app instance to the correct port on the db instance, using the latter's IP. For improved quality of life, put hte export command into your `.bashrc` folder to make the environment variable persistent

- With the connection to the database instance secured via `DB_HOST` and the updated IP in the `default` file, restart and re-enable `nginx`.

- All that remains is to switch to your `app` folder, run `node seeds/seed.js` and then install and start `npm`. Congratulations, your app now runs in the cloud.


