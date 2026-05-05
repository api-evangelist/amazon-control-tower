---
title: "Automating Cost Optimization Governance with AWS Config"
url: "https://aws.amazon.com/blogs/mt/automating-cost-optimization-governance-with-aws-config/"
date: "Fri, 07 Feb 2025 16:20:59 +0000"
author: "Matt King"
feed_url: "https://aws.amazon.com/blogs/mt/category/management-tools/aws-control-tower/feed/"
---
<h2>Overview</h2> 
<p>A key benefit of using the Amazon Web Services (AWS) cloud is the ability to pay only for the services you consume.&nbsp;This granular control and elastic model enables you to <a href="https://aws.amazon.com/blogs/aws-cloud-financial-management/five-things-you-should-do-to-create-an-accurate-on-premises-vs-cloud-comparison-model/#:~:text=Numerous%20independent%20and%20AWS%2Dled,to%20running%20on%20premises%20infrastructure.">achieve substantial savings compared to on-premise infrastructure</a>. The practice of ensuring you are getting the most value for your investment, and a foundational pillar of the <a href="https://aws.amazon.com/architecture/well-architected/?wa-lens-whitepapers.sort-by=item.additionalFields.sortDate&amp;wa-lens-whitepapers.sort-order=desc&amp;wa-guidance-whitepapers.sort-by=item.additionalFields.sortDate&amp;wa-guidance-whitepapers.sort-order=desc">Well-Architected Framework</a>, is Cost Optimization.</p> 
<p>Cost optimization has long been considered a retrospective exercise at the end of each month undertaken by the Finance team, but that narrative is no longer applicable. It is a shared responsibility that needs to be collectively owned by everyone on an ongoing basis. In order to achieve the most value, cost optimization should be implemented from both a strategic and tactical perspective. Strategic being the combination of best practice, the use of cloud native services, and a data driven approach. The tactical perspective relates to the specific proactive actions that can drive immediate savings. Combining these approaches, but more importantly, establishing and maintaining an optimum baseline standard in an automated manner will maximise your business value, and the price performant efficiency of your workloads in the AWS cloud.</p> 
<p><a href="https://aws.amazon.com/config/">AWS Config</a> is a service that continually assesses, audits, and evaluates the configurations and relationships of your resources on AWS, on premises, and on other clouds.&nbsp;In this post, we show you how to enhance the value of the AWS Config service by deploying a solution across your organization that automatically evaluates resources against best practice logic for cost optimization utilizing a feature called <a href="https://docs.aws.amazon.com/config/latest/developerguide/conformance-packs.html">Conformance Packs for AWS Config</a>.</p> 
<p><strong>What are conformance packs and their benefits?</strong></p> 
<p>A conformance pack is a collection of AWS Config rules and remediation actions that provide a general-purpose compliance framework to help you codify and deploy strategic security, operational or cost-optimization governance checks in a single account and Region, or at scale across an entire <a href="https://aws.amazon.com/organizations/">AWS Organization</a>. These rules will automatically monitor and evaluate your resources to identify their compliance posture against the logic defined in each rule. AWS Config provides <a href="https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_use-managed-rules.html">AWS Config Managed Rules,</a> a list of predefined customizable rules or <a href="https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_develop-rules.html">AWS Config Custom Rules</a> where you define your own custom logic using <a href="https://aws.amazon.com/lambda/">AWS Lambda</a> functions or <a href="https://github.com/aws-cloudformation/cloudformation-guard">AWS CloudFormation Guard</a>, a policy-as-code-language.</p> 
<p>When a resource fails to comply with the logic defined in a rule, it is marked as <strong>Noncompliant.</strong> You have the option to manually or automatically invoke tactical remediation steps using <a href="https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-automation.html">AWS Systems Manager Automation,</a> a service which provides an operations hub for your AWS applications and resources, with predefined runbooks to automatically remediate non-compliant resources or trigger event driven workflows such as alerting a team to take action.</p> 
<h2>Solution Overview</h2> 
<p>The Cost Optimization Conformance Pack solution empowers customers already utilizing AWS Config to maximize their service investment by integrating cost optimization governance for enhanced service value. The customizable solution includes a collection of three example custom rules containing best practice cost optimization logic. These will monitor and evaluate your resources to identify their cost optimization compliance posture and replicate the results back to an <a href="https://docs.aws.amazon.com/config/latest/developerguide/aggregate-data.html">AWS Config aggregator</a> in a single ‘<a href="https://docs.aws.amazon.com/organizations/latest/userguide/orgs_integrate_delegated_admin.html">delegated administrator</a>‘ account for simplified management and reporting.&nbsp;The following rules are included:</p> 
<p><strong>Rule 1</strong>: Check for EBS gp2 volumes&nbsp;<strong>Remediation</strong>: Convert them to gp3 volumes.</p> 
<p><strong>Rule 2</strong>: Check for EBS volumes not attached to an EC2 instance.</p> 
<p><strong>Rule 3:</strong> Check for S3 buckets that do not have a lifecycle configuration policy.</p> 
<p>If a resource does not meet any of the criteria defined in a rule then the resource, config rule and conformance pack will all be marked as <strong>Noncompliant.</strong> Rule 1 also contains a remediation action that can be invoked manually or automatically, triggering an SSM automation runbook to convert the EBS volume from gp2 to gp3.&nbsp;With gp3 volumes, you can provision IOPS and throughput independently, without increasing storage size, at costs up to 20% lower per GB compared to gp2 volumes.</p> 
<p><em>Note</em>: AWS Config is a chargeable service and you should refer to the <a href="https://aws.amazon.com/config/pricing/">pricing examples</a> to understand the cost implications of enabling the service across your organization if you do not already use it.</p> 
<p>The solution can be deployed across an AWS Organization with or without <a href="https://aws.amazon.com/controltower/">AWS Control Tower</a> enabled. AWS Control Tower is an orchestration solution that simplifies the set up and governance of an AWS multi-account environment, following prescriptive best practices. Figure 1 refers to the deployment of the Cost Optimization Conformance Pack across an example OU structure provided by AWS Control Tower with member accounts.</p> 
<p><img alt="Architecture diagram for the Cost Optimization Conformance Pack solution showing an AWS Organization using AWS Control Tower with three Organizational Units and member accounts. The conformance pack is deployed from the audit account within the Security OU into designated member accounts and results are reported back to the AWS Config Aggregator within that account." class="wp-image-58914 size-large aligncenter" height="782" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/01/06/Figure-1-Architecture-1024x782.png" width="1024" /></p> 
<p><strong>Figure 1: Cost Optimization Conformance Pack architecture</strong></p> 
<p>Within the Security OU, we will utilize the audit account and register it as a delegated administrator for AWS Config and <a href="https://aws.amazon.com/cloudformation/">AWS CloudFormation</a>. This will grant it permissions to deploy the Cost Optimization Conformance Pack and associated resources defined in the CloudFormation Stack across the accounts in your organization.</p> 
<p>The CloudFormation Stack contains the Cost Optimization Conformance Pack custom rules along with an&nbsp;<a href="https://aws.amazon.com/lambda/">AWS Lambda</a> function and an AWS Systems Manager document that are referenced by the rules. To enable the execution of the Lambda function and Systems Manager document, two custom&nbsp;<a href="https://aws.amazon.com/iam/">AWS Identity and Access Management</a> (IAM) roles are created using <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/what-is-cfnstacksets.html">CloudFormation StackSets</a>. These resources and roles are deployed to all accounts within the Organization from the centralised audit account to simplify the management of the solution.</p> 
<p>AWS Config can check your resources for compliance against the conformance pack rules periodically or in response to configuration changes in your environment. The&nbsp;AWS Config Aggregator in the audit account will collate and centralize the data being captured in each of your member accounts by AWS Config, enabling further analysis on a per region basis.</p> 
<h2>Prerequisites</h2> 
<p>This post assumes you already have <a href="https://aws.amazon.com/organizations/">AWS Organizations</a> and <a href="https://docs.aws.amazon.com/controltower/latest/userguide/getting-started-with-control-tower.html">AWS Control Tower enabled</a> which deploys the OU structure, audit account and AWS Config Aggregator. If you do not plan to use AWS Control Tower then refer to the guides on&nbsp;<a href="https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_org_create.html">how to create an AWS Organization</a> and <a href="https://docs.aws.amazon.com/config/latest/developerguide/aggregated-create.html">creating aggregators for AWS Config</a> before proceeding.</p> 
<p>In addition, you will need:</p> 
<ul> 
 <li>Permission to access both your organizations management account and the audit account you are delegating administrative rights to for deployment of the Cost Optimization Conformance Pack solution.</li> 
 <li>Trusted access for StackSets with AWS Organizations enabled. Steps for checking this are provided in the <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-orgs-activate-trusted-access.html">Activate trusted access for stack sets with Organizations</a> documentation.</li> 
 <li>AWS Console access to AWS Config in the member accounts where the solution is being deployed.</li> 
</ul> 
<h2>Walkthrough</h2> 
<p>This post will walk you through completing the following steps:</p> 
<ul> 
 <li>Establish a trust relationship between AWS Organizations and the service principals for AWS Config and AWS CloudFormation&nbsp;StackSets.</li> 
 <li>Grant ‘delegated administrator’ permissions for the AWS Config and AWS CloudFormation services to the audit account. 
  <ul> 
   <li>If Control Tower is not enabled, use the account where the AWS Config aggregator has been deployed for the procedure steps that refer to the ‘audit’ account.</li> 
  </ul> </li> 
 <li>Deploy the Cost Optimization Conformance Pack. 
  <ul> 
   <li>Using CloudFormation, you will deploy the conformance pack, Lambda function, IAM roles and Systems Manager document included in the solution.</li> 
  </ul> </li> 
 <li>Testing the solution.</li> 
</ul> 
<p>For the purposes of this walkthrough, our management account ID will be <em>111111111111</em> and the audit account from which we will deploy the solution will have an account ID of <em>222222222222</em>.</p> 
<h2>Deploy the solution</h2> 
<p><strong>Establish a trust relationship between AWS Organizations and the service principals for AWS CloudFormation</strong></p> 
<p>The trust relationship between AWS Organizations and AWS Config will already be partially established as per the prerequisite steps. To create the additional trust relationships for AWS Config rules and AWS CloudFormation, run the following CLI commands using AWS CloudShell in the organization management account:</p> 
<p><strong>To create and validate the trust relationships (console)</strong></p> 
<ol> 
 <li>Open the AWS Console.</li> 
 <li>Login to your Organization management account as administrator.</li> 
 <li>Type <strong>CloudShell</strong> in the console Search bar and select <strong>CloudShell</strong>.</li> 
 <li>This will launch a CloudShell terminal window within the browser at the bottom of your screen.</li> 
 <li>Run the following commands <code>aws organizations enable-aws-service-access --service-principal=config-multiaccountsetup.amazonaws.com</code> and <code>aws organizations enable-aws-service-access --service-principal=member.org.stacksets.cloudformation.amazonaws.com</code></li> 
</ol> 
<ol start="6"> 
 <li>Validate the trust relationships have been correctly established with the following command: <code>aws organizations list-aws-service-access-for-organization</code></li> 
 <li>The output should contain the values shown in Figure 2:</li> 
</ol> 
<p><img alt="AWS CLI showing the service principals with an established trust relationship with AWS Organizations." class="alignnone wp-image-58915 size-large" height="412" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/01/06/Figure-2-Trust-Relationships-1024x412.png" width="1024" /></p> 
<p><strong>Figure 2: AWS Organizations service principal trust relationships</strong></p> 
<p><strong>Enable a ‘Delegated Administrator’ account for AWS Config</strong></p> 
<p>This step will enable the audit account as a delegated administrator for the AWS Config service and Config rules. Delegated administrators are accounts within a given AWS Organization that are granted additional administrative privileges, in this case for the AWS Config service to deploy and manage rules across accounts.</p> 
<p><strong>To setup a delegated administrator account for AWS Config (console)</strong></p> 
<ol> 
 <li>Repeat the steps to login to <strong>CloudShell.</strong></li> 
 <li>Run the <code>aws organizations register-delegated-administrator --account-id 222222222222&nbsp;--service-principal config-multiaccountsetup.amazonaws.com</code> command replacing the account-id with the ID of the audit account.</li> 
 <li>Run the <code>aws organizations register-delegated-administrator --account-id 222222222222&nbsp;--service-principal config.amazonaws.com</code> command replacing the account-id with the ID of the audit account.</li> 
 <li>To check the audit account has been successfully established as a delegated administrator for AWS config, run the following commands: <code>aws organizations list-delegated-administrators --service-principal=config.amazonaws.com</code> and <code>aws organizations list-delegated-administrators --service-principal=config-multiaccountsetup.amazonaws.com</code></li> 
</ol> 
<ol start="5"> 
 <li>You should see output similar to Figure 3 for each command with the account ID of your delegated administrator account.</li> 
</ol> 
<p><img alt="AWS CLI showing the delegated administrator for the AWS Config service principal." class="alignnone size-large wp-image-58916" height="240" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/01/06/Figure-3-Delegated-Administrators-1024x240.png" width="1024" /></p> 
<p><strong>Figure 3: AWS Organizations delegated administrators</strong></p> 
<p><strong>Enable a ‘Delegated Administrator’ account for AWS CloudFormation</strong></p> 
<p>To grant delegated administrator permissions to the audit account for AWS CloudFormation, we need to use&nbsp;<a href="https://docs.aws.amazon.com/organizations/latest/userguide/services-that-can-integrate-cloudformation.html%20">AWS CloudFormation StackSets and AWS Organizations</a>. This feature creates the required IAM roles in each member account to support the deployment of the CloudFormation StackSet containing the Cost Optimization Conformance Pack resources across the organization.</p> 
<p><strong>To setup a delegated administrator account for AWS CloudFormation (console)</strong></p> 
<ol> 
 <li>Repeat the steps to login to <strong>CloudShell.</strong></li> 
 <li>Run the <code>aws organizations register-delegated-administrator&nbsp; --service-principal=member.org.stacksets.cloudformation.amazonaws.com&nbsp; --account-id=222222222222</code> command replacing the account-id with the ID of the audit account.</li> 
 <li>To check the audit account has been successfully established as a delegated administrator for AWS config, run the following command: <code>aws organizations list-delegated-administrators&nbsp;&nbsp;--service-principal=member.org.stacksets.cloudformation.amazonaws.com</code></li> 
 <li>You should see output similar to Figure 3 with the account ID of your delegated administrator account.</li> 
</ol> 
<p><strong>Deploy the Cost Optimization Conformance Pack</strong></p> 
<p>In this step, you will download and deploy the conformance pack in the audit account using <a href="https://github.com/aws-samples/aws-config-cost-optimization-conformance-pack/blob/main/template.yaml">this CloudFormation&nbsp;YAML template</a> from the <a href="https://github.com/aws-samples/aws-config-cost-optimization-conformance-pack">AWS Samples Cost Optimization Conformance Pack GitHub repository</a>. CloudFormation StackSets simplify the process for deployment and management of AWS resources.</p> 
<p>For this solution the following will be deployed:</p> 
<ul> 
 <li><strong>AWS Config Organization Conformance Pack</strong> – a collection of AWS Config custom rules that will be used to evaluate resources against best practice cost optimization logic. 
  <ul> 
   <li>This Organization Conformance Pack will deploy individual Cost Optimization Conformance Packs into each member account.</li> 
  </ul> </li> 
 <li><strong>AWS CloudFormation StackSet</strong> – a collection of CloudFormation stacks deployed into all the member accounts in the AWS Organization. These stacks will deploy the following: 
  <ul> 
   <li><strong>AWS Lambda Function</strong>&nbsp;– The AWS Config custom rules invoke a Lambda function&nbsp;that contains the logic to evaluate whether the specified resource is either <strong>Compliant</strong> or <strong>Noncompliant</strong> with cost optimization best practice rules defined above.</li> 
   <li><strong>IAM Roles</strong> – Two custom IAM roles will be deployed. One that will enable the Lambda function to be invoked and the second which will be used by AWS Systems Manager (SSM) to carry out remediation actions as defined in the SSM document.</li> 
  </ul> </li> 
 <li><strong>AWS Systems Manager Automation Document –</strong> This will be deployed into the audit account only and used by the member accounts.</li> 
</ul> 
<p><strong>To download and deploy the Cost Optimization Conformance Pack (console)</strong></p> 
<ol> 
 <li>Repeat the steps to login to <strong>CloudShell.</strong></li> 
 <li>Upload the YAML file to the CloudShell session by clicking the <strong>Upload file</strong> icon and selecting the YAML file you downloaded.</li> 
 <li>Run the <code>aws cloudformation deploy --template-file template.yaml --stack-name CostOptimizationConfPack --parameter-overrides DeployingInDelegatedAdminAccount=True&nbsp;--capabilities CAPABILITY_IAM</code> command.</li> 
 <li>To verify the CloudFormation StackSet has been deployed successfully type <strong>CloudFormation</strong> in the console Search bar and select <strong>CloudFormation</strong>.</li> 
 <li>Select Stacks on the left menu. The output should show the Stack that has been deployed with a <strong>CREATE_COMPLETE</strong>&nbsp;status as shown in Figure 4.</li> 
 <li>To verify the conformance pack has been deployed successfully type <strong>AWS Config</strong> in the console Search bar and select <strong>AWS Config</strong>.</li> 
 <li>Select <strong>Conformance packs</strong> on the left menu. Your dashboard should look similar to the one in Figure 5.</li> 
</ol> 
<p><img alt="Under CloudFormation Stacks, the status of the stack deployment shows CREATE_COMPLETE meaning it has been successful. " class="alignnone size-large wp-image-58917" height="310" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/01/06/Figure-4-CloudFormation-Stack-Deployment-1024x310.png" width="1024" /></p> 
<p><strong>Figure 4: CloudFormation Stack deployment status</strong></p> 
<p>The AWS Config Conformance packs dashboard shows that it has successfully deployed and is reporting a Compliance score. This score indicates a percentage compliance level for the resources that have been evaluated against the rules in the conformance pack. This dashboard will typically report <strong>INSUFFICIENT DATA</strong> until the rules have been evaluated for the first time.</p> 
<p><img alt="The Cost Optimization Conformance Pack shows a deployment status of ‘Completed’ under AWS Config Conformance packs. " class="alignnone size-large wp-image-58918" height="379" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/01/06/Figure-5-Conformance-Pack-Deployment-1024x379.png" width="1024" /><img alt="" src="media/image5.png" /></p> 
<p><strong>Figure 5: Conformance pack deployment status</strong></p> 
<h3></h3> 
<h2>Testing the solution</h2> 
<h3><strong>Check the Cost Optimization Conformance Pack status</strong></h3> 
<p>Now that the conformance pack and associated resources have been deployed across your accounts, you can see the status of the conformance pack rules using the AWS Config Conformance packs dashboard. You may already have resources that are being evaluated against the rules that are showing as either <strong>Complaint</strong> or <strong>Noncompliant</strong> as Figure 6 demonstrates. If there are no resources showing, follow the step below to create a test Noncompliant Amazon EBS volume.</p> 
<p><img alt="The Cost Optimization Conformance Pack status shows the Rules included in the pack and their compliance status which is Noncompliant in this example." class="alignnone size-large wp-image-58919" height="568" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/01/06/Figure-6-Check-Conformance-Pack-Status-1024x568.png" width="1024" /></p> 
<p><strong>Figure 6: Cost Optimization Conformance Pack status</strong></p> 
<p><strong>To test the conformance pack with a Noncompliant resource (console)</strong></p> 
<ol> 
 <li>Open the AWS Console.</li> 
 <li>Login to a member account within your organization as administrator.</li> 
 <li>Create an Amazon EBS volume using <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-creating-volume.html">this procedure</a>, ensuring you select the <strong>Volume Type</strong>&nbsp;as <strong>gp2</strong>.</li> 
 <li>Once the EBS volume has been created navigate to the AWS Config dashboard.</li> 
 <li>Select <strong>Conformance packs</strong>.</li> 
 <li>Select the conformance pack with <strong>CostOptimization</strong> in the name.</li> 
 <li>A list of <strong>Rules</strong> will appear as shown in Figure 6.</li> 
 <li>Select the rule with <strong>CostOpt-Ebs</strong> in the name to view the rule dashboard.</li> 
 <li>Select the <strong>Actions</strong> menu.</li> 
 <li>Select <strong>Re-evaluate</strong> to trigger the rule to assess the resources in the account.</li> 
</ol> 
<p>After the evaluation has completed the Noncompliant EBS volume will display as shown in Figure 7. Clicking the volume ID will provide more information about the resource.</p> 
<p><img alt="AWS Config Rules shows the status of individual rules and the resources that have been detected as either compliant or noncompliant. This example shows EBS volumes that are noncompliant as they are volume type gp2." class="alignnone size-large wp-image-58920" height="469" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/01/06/Figure-7-Config-Rule-Status-1024x469.png" width="1024" /></p> 
<p><strong>Figure 7: Config rule status</strong></p> 
<h3>Testing Remediation</h3> 
<p>If a rule has a remediation action configured, it is possible to invoke this from the AWS Console view as shown in Figure 7. In the case of the EBS gp2 rule, this will invoke the Systems Manager automation runbook in the audit account to convert the volume type to gp3.</p> 
<p><strong>To invoke the remediation rule (console)</strong></p> 
<ol> 
 <li>From the AWS Config rule dashboard in Figure 7 scroll down to <strong>Resources in scope</strong>.</li> 
 <li>Select the radio button for the <strong>EC2 Volume</strong> that is listed as <strong>Noncompliant</strong>.</li> 
 <li>Select the Remediate button.</li> 
</ol> 
<p>Once the remediation rule has been triggered <strong>Action executed successfully</strong> should then appear under the <strong>Status</strong> column. You can also validate the change in volume type has completed successfully by looking at the volumes listed in the Elastic Block Store under the EC2 service. Figure 8 shows the volume <strong>Type</strong> is now <strong>gp3</strong>.</p> 
<p><img alt="Under EC2 Volumes the list of provisioned EBS volumes for the region is displayed. The volume type shows one of the four instances is now gp3 after the remediation action in the AWS Config rule was triggered to invoke the conversion from type gp2." class="alignnone size-large wp-image-58921" height="124" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/01/06/Figure-8-EBS-Volume-Status-1024x124.png" width="1024" /></p> 
<p><strong>Figure 8: EBS volumes status</strong></p> 
<p>Finally, AWS Config will update the status of the rule to show that the EBS volume which has been converted to gp3 is now showing as Compliant.</p> 
<p><em>Note: The update of this status is not real-time.</em></p> 
<p><img alt="AWS Config Rules shows the status of individual rules and the resources that have been detected as either compliant or noncompliant. This example shows EBS volumes that are both compliant as they are type gp3 and noncompliant as they are type gp2. " class="alignnone size-large wp-image-58922" height="496" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/01/06/Figure-9-Config-Rule-Status-with-Compliant-Volume-1024x496.png" width="1024" /></p> 
<p><strong>Figure 9: Config rule status with compliant resource</strong></p> 
<h2></h2> 
<h2>Clean Up</h2> 
<p>All the resources deployed for the Cost Optimization Conformance Pack solution can be removed by deleting the CloudFormation stack either through the AWS Console, or using the CLI command below:</p> 
<p><strong>To delete the Cost Optimization Conformance Pack solution (CLI)</strong></p> 
<ol> 
 <li>Repeat the steps to login to <strong>CloudShell.</strong></li> 
 <li>Run the <code>aws cloudformation delete-stack&nbsp;--stack-name CostOptimizationConfPack</code>&nbsp;command.</li> 
</ol> 
<p>You can also deregister the audit account as a delegated administrator for AWS Config and CloudFormation from Organizations by replacing the <strong>account-id</strong> with the ID for the audit account and running the CLI commands below.</p> 
<ol start="3"> 
 <li>Run the <code>aws organizations deregister-delegated-administrator --account-id 123412341234&nbsp;--service-principal config-multiaccountsetup.amazonaws.com</code> and <code>aws organizations deregister-delegated-administrator --account-id 123412341234 --service-principal config.amazonaws.com</code> commands.</li> 
</ol> 
<h2></h2> 
<h2>Conclusion</h2> 
<p>The AWS Config Cost Optimization Conformance Pack is a powerful yet simple way to define, automate and govern cost optimization standards across your organization. The solution incorporates some best practice cost optimization rules to help establish and monitor a consistent compliance posture with the ability to invoke remediation steps for non-compliant resources.</p> 
<p>The Cost Optimization Conformance Pack is an open source solution available from the <a href="https://github.com/aws-samples/aws-config-cost-optimization-conformance-pack">AWS Samples GitHub repository</a>. We encourage you to incorporate your own cost optimization rules and remediation’s into the conformance pack using the <a href="https://github.com/aws-samples/aws-config-cost-optimization-conformance-pack/blob/main/README.md">README guide</a> available in the repository. You can find out more about <a href="https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_develop-rules_nodejs.html">AWS Config custom Lambda rules</a> and <a href="https://docs.aws.amazon.com/systems-manager/latest/userguide/documents.html">AWS Systems Manager documents</a> to expand the functionality of the solution further.</p> 
<p>See the <a href="https://aws.amazon.com/blogs/mt/customize-aws-config-resource-tracking-in-aws-control-tower-environment/">Customize AWS Config resource tracking in AWS Control Tower environment</a> post for further reading around fine tuning AWS Config.</p>
