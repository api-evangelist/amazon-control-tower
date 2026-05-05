---
title: "Scaling AWS Control Tower controls using Amazon Bedrock Agents"
url: "https://aws.amazon.com/blogs/mt/scaling-aws-control-tower-controls-using-amazon-bedrock-agents/"
date: "Thu, 31 Oct 2024 17:40:06 +0000"
author: "Akhil Raj Yallamelli"
feed_url: "https://aws.amazon.com/blogs/mt/category/management-tools/aws-control-tower/feed/"
---
<p><a href="https://aws.amazon.com/controltower/">AWS Control Tower</a> is the easiest way to set up and govern a security, multi-account AWS environment. A key feature of AWS Control Tower is to deploy and manage controls at scale across an entire <a href="https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html">AWS Organizations</a>. These controls are categorized based on their behavior and guidance. The behavior of each control is one of <a href="https://docs.aws.amazon.com/controltower/latest/controlreference/control-behavior.html">detective, proactive or preventive</a>.</p> 
<p>This blog post shows you how to use <a href="https://aws.amazon.com/bedrock/agents/">Amazon Bedrock Agents</a> to automate the enablement and disablement process for <a href="https://docs.aws.amazon.com/controltower/latest/controlreference/introduction.html">AWS Control Tower Controls</a>. By leveraging Amazon Bedrock Agents and <a href="https://docs.aws.amazon.com/controltower/latest/controlreference/control-api-examples-short.html">Control Tower Control APIs</a>, users can interact through the chat interface while providing the necessary parameters, allowing the agent to handle the API calls, thus ensuring efficiency and accuracy.</p> 
<p>A control provides instructions for configuring resources to mitigate or address specific risks i.e. assist you in expressing your policy intentions. For example, if you need to enable a detective control that detects <a href="https://docs.aws.amazon.com/controltower/latest/controlreference/strongly-recommended-controls.html#ssh-disallow-internet">whether unrestricted internet connection through SSH is allowed</a>, you can enable this control and other controls through a single action using the Management Console or AWS Command Line (AWS CLI), AWS SDK, or Infrastructure as Code(IaC).</p> 
<h2><strong>Walkthrough</strong></h2> 
<p>Before we dive deep into the deployment, let’s walkthrough the key steps of the architecture as shown in Figure 1 below.</p> 
<p><a href="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2024/10/18/controls.png"><img alt="Archicture diagram of the solution" class="aligncenter wp-image-57230 size-full" height="772" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2024/10/18/controls.png" width="1826" /></a></p> 
<p style="text-align: center;"><em>Figure 1. Architecture diagram</em></p> 
<ol> 
 <li>The user interacts with Amazon Bedrock Agent via <a href="https://docs.aws.amazon.com/bedrock/latest/userguide/agents-test.html">Test window</a>, that supports actions such as enable, disable, list or enabled controls status information.</li> 
 <li>The agent uses <a href="https://aws.amazon.com/bedrock/knowledge-bases/">Amazon Bedrock Knowledge Base</a> to retrieve detailed information about each control and its <a href="https://docs.aws.amazon.com/controltower/latest/controlreference/control-identifiers.html">API control Identifier</a>.</li> 
 <li>The Amazon Bedrock Agent triggers the action group based on user’s request. The Lambda functions associated with the action group perform the actions by calling <a href="https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/controltower.html">AWS Control Tower APIs</a> with the required parameters. AWS Control Tower executes the following actions and manages the controls as specified by the Amazon Bedrock Agent. 
  <ul> 
   <li><strong>Find all relevant controls</strong>: Find all relevant control identifiers for a use-case.</li> 
   <li><strong>List enabled controls</strong>: Retrieves a list of currently enabled controls for a specific Region and <a href="https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_ous.html">Organizational Unit</a> (OU) using Control Tower <a href="https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/controltower/client/list_enabled_controls.html">list_enabled_controls</a> API.</li> 
   <li><strong>Enable/disable control</strong>: Handles requests to enable or disable a control for a particular Region and OU using AWS Control Tower <a href="https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/controltower/client/enable_control.html">enable_contro</a>l and <a href="https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/controltower/client/disable_control.html">disable_control</a> API’s respectively.</li> 
   <li><strong>Bulk enable/disable controls</strong>: Handles requests to enable or disable a set of controls for multiple OU’s using AWS Control Tower <a href="https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/controltower/client/enable_control.html">enable_contro</a>l and <a href="https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/controltower/client/disable_control.html">disable_control</a> API’s respectively.</li> 
   <li><strong>Get enabled control status/information</strong>: Handles requests to get enabled control status/information using <a href="https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/controltower/client/get_enabled_control.html">get_enabled_control</a> API.</li> 
  </ul> </li> 
</ol> 
<h2><strong>Prerequisites</strong></h2> 
<ol> 
 <li>An existing AWS environment with AWS Control Tower enabled.</li> 
 <li>Access to the AWS management account.</li> 
 <li>An account where the solution will be deployed, e.g. GenAI tooling account.</li> 
 <li>Access to Titan Embeddings G1 – Text and Anthropic Claude foundation models in Amazon Bedrock.</li> 
</ol> 
<h2><strong>Deployment Steps</strong></h2> 
<p>The deployment of the solution consists of four steps:</p> 
<ol> 
 <li>Configuring Amazon Bedrock knowledge base.</li> 
 <li>Setting up Amazon Bedrock Agent.</li> 
 <li>Configuring actions groups.</li> 
 <li>Setting up AWS Identity and Access Management (IAM) permissions.</li> 
</ol> 
<h3><strong>&nbsp;</strong><strong>Step 1: Configuring </strong><strong>Amazon Bedrock knowledge base</strong><strong>: </strong></h3> 
<ul> 
 <li>In an AWS account (e.g. GenAI Tooling Account) where you plan to deploy this solution, log in to the&nbsp;<a href="https://console.aws.amazon.com/s3">Amazon S3 console</a>&nbsp;and navigate to&nbsp;<strong>Buckets</strong>&nbsp;on left pane. Select&nbsp;<strong>Create bucket</strong>. Provide a unique name and use the default settings. Click&nbsp;<strong>Create bucket</strong>.</li> 
 <li>Upload the&nbsp;<a href="https://github.com/aws-samples/scaling-aws-control-tower-controls-using-amazon-bedrock-agents/blob/main/knowledge-base/ct-api-controlidentifiers.json">json file</a> to the newly created S3 bucket. This file contains a list of unique API Control identifiers and description for each control.</li> 
 <li>Log in&nbsp;<a href="https://console.aws.amazon.com/bedrock">Amazon Bedrock console</a>&nbsp;and navigate to the&nbsp;<strong>Knowledge bases (KB)</strong>&nbsp;under <strong>Builder tools</strong> on the left. Select&nbsp;<strong>Create knowledge base</strong>.</li> 
 <li>Provide a&nbsp;<strong>Knowledge base name </strong>and an optional&nbsp;<strong>Knowledge base description</strong>&nbsp;that reflects the purpose of your KB.</li> 
 <li>In the&nbsp;<strong>IAM permissions </strong>section, select first option i.e.&nbsp;<strong>Create and use new service role</strong>. Provide a&nbsp;<strong>Service role name</strong>. This pre-configured IAM role has the required permissions. Choose Amazon S3 as data source. Provide an optional tag and click&nbsp;<em><strong>Next</strong></em>.</li> 
 <li>Provide a&nbsp;<strong>Data source name</strong>. Select&nbsp;<strong>Data source location </strong>to be&nbsp;<em>This AWS account.</em></li> 
 <li>Provide the&nbsp;<strong>S3 URI </strong>of the object you uploaded to the bucket in step (ii).</li> 
 <li>Select the&nbsp;<strong>embeddings model </strong>to be&nbsp;<em>Titan Embeddings G1 – Text by Amazon</em>. This model is pre-configured and ready to use.</li> 
 <li>For the&nbsp;<strong>Vector database</strong>, select the recommended option&nbsp;<em>Quick create a new vector store</em>. Select <strong>Next, </strong>then review and <strong>Create knowledge base</strong>.</li> 
</ul> 
<h3><strong>Step 2: Setting up Amazon Bedrock Agent:</strong></h3> 
<ul> 
 <li>Open the&nbsp;<a href="https://console.aws.amazon.com/bedrock">Amazon Bedrock console</a>, select&nbsp;<strong>Agents</strong>&nbsp;under <strong>Builder tools </strong>on the left navigation panel, then select&nbsp;<strong>Create Agent</strong>. Provide agent details including&nbsp;<strong>Name </strong>and an optional&nbsp;<strong>description </strong>then<strong> Create</strong>.</li> 
 <li>Under&nbsp;<strong>Agent resource role </strong>section, select the first option&nbsp;<strong>Create and use a new service role</strong>. This (IAM) service role gives your agent access to required services, such as AWS Lambda.</li> 
 <li>In&nbsp;<strong>Select model </strong>section, choose&nbsp;<strong>Anthropic </strong>and<strong> Claude V2.1</strong></li> 
 <li>In<strong> Instructions for the Agent, </strong>use the instructions provided <a href="https://github.com/aws-samples/scaling-aws-control-tower-controls-using-amazon-bedrock-agents/blob/main/agent-instructions/instructions.txt">here</a>.</li> 
 <li>In <strong>Knowledge bases </strong>section, select <strong>Add</strong> then use the knowledge base created in the above step. This will help in finding control identifiers using a description.</li> 
 <li>Leave rest of the sections with default configuration. Next, click&nbsp;<strong>Save </strong>and<strong> exit</strong>.</li> 
</ul> 
<h3><strong>Step 3: Configuring Action Groups:</strong></h3> 
<p>We need to create a S3 bucket for storing <a href="https://www.openapis.org/">OpenAPI</a> schemas that is required for creating the action groups.</p> 
<ul> 
 <li>Log in&nbsp;<a href="https://console.aws.amazon.com/s3">Amazon S3 console</a>&nbsp;and navigate to&nbsp;<strong>Buckets</strong>&nbsp;on left pane. Select&nbsp;<strong>Create bucket</strong>. Provide a unique name and use the default settings. Click&nbsp;<strong>Create bucket</strong>.</li> 
 <li>Upload the OpenAPI JSON files for five action groups – <em>Action Group </em>to<em> Find all relevant controls (</em><a href="https://github.com/aws-samples/scaling-aws-control-tower-controls-using-amazon-bedrock-agents/blob/main/find-all-relevant-controls/OpenAPI-schema.json">OpenAPI-schema</a><em>)</em>, <em>List enabled controls (</em><a href="https://github.com/aws-samples/scaling-aws-control-tower-controls-using-amazon-bedrock-agents/blob/main/list-enabled-controls/OpenAPI-schema.json">OpenAPI-schema</a><em>),</em> <em>Enable/disable control (</em><a href="https://github.com/aws-samples/scaling-aws-control-tower-controls-using-amazon-bedrock-agents/blob/main/enable-disable-control/OpenAPI-schema.json">OpenAPI-schema</a><em>)</em>,&nbsp; <em>Bulk enable/disable controls</em> (<a href="https://github.com/aws-samples/scaling-aws-control-tower-controls-using-amazon-bedrock-agents/blob/main/bulk-enable-disable-controls/OpenAPI-schema.json">OpenAPI-schema</a>) and <em>Get enabled control status/information</em> (<a href="https://github.com/aws-samples/scaling-aws-control-tower-controls-using-amazon-bedrock-agents/blob/main/get-enable-control-status/OpenAPI-schema.json">OpenAPI-schema</a>)</li> 
</ul> 
<p><strong>Note:</strong> These files contain the schema that outlines the API description, structure, and parameters for the action group. The <a href="https://docs.aws.amazon.com/bedrock/latest/userguide/agents-api-schema.html">OpenAPI schemas</a> manage the logic for receiving user inputs and triggering the corresponding Lambda functions.</p> 
<ul> 
 <li>Navigate to&nbsp;<a href="https://console.aws.amazon.com/bedrock">Amazon Bedrock console&nbsp;</a>. Select the agent created in previously. Click&nbsp;<strong>Edit in Agent Builder</strong>.</li> 
 <li>In&nbsp;<strong>Action groups</strong>&nbsp;section, click&nbsp;<strong>Add</strong>.</li> 
 <li>Provide&nbsp;<em>Find all relevant controls</em> as&nbsp;<strong>Action group name</strong>. For Action group type, select&nbsp;<strong>Define with API schemas</strong>. For Action group invocation, select&nbsp;<strong>Quick create a lambda function-<em>recommended</em></strong>&nbsp;option.</li> 
 <li>For action group schema section, click&nbsp;<strong>Select an existing API schema</strong>. Provide the path for S3 url for this action group created in previous step. Click&nbsp;<strong>Save and exit</strong></li> 
 <li>Add other four action groups List enabled controls, Enable/disable control, Bulk enable/disable controls, Get enabled control status by following similar steps.</li> 
 <li>Navigate to&nbsp;<a href="https://console.aws.amazon.com/lambda">AWS Lambda console&nbsp;</a>. On the left, select <strong>Functions</strong>.</li> 
 <li>Click&nbsp;<em>Find all relevant controls</em> function. Update the code from linked <a href="https://github.com/aws-samples/scaling-aws-control-tower-controls-using-amazon-bedrock-agents/blob/main/find-all-relevant-controls/lambda_function.py">repository</a> and click deploy.</li> 
 <li>Next, click second lambda function – <em>List enabled controls. </em>Update the code from linked <a href="https://github.com/aws-samples/scaling-aws-control-tower-controls-using-amazon-bedrock-agents/blob/main/list-enabled-controls/lambda_function.py">repository</a> and click deploy.</li> 
 <li>Next, click third lambda function – <em>Enable/disable control</em>. Update the code from linked <a href="https://github.com/aws-samples/scaling-aws-control-tower-controls-using-amazon-bedrock-agents/blob/main/enable-disable-control/lambda_function.py">repository</a> and click deploy.</li> 
 <li>Next, click <em>Bulk enable/disable controls</em> function<em>. </em>Update the code from linked <a href="https://github.com/aws-samples/scaling-aws-control-tower-controls-using-amazon-bedrock-agents/blob/main/bulk-enable-disable-controls/lambda_function.py">repository</a> and click deploy.</li> 
 <li>Finally, click <em>Get enabled control status</em> function<em>. </em>Update the code from linked <a href="https://github.com/aws-samples/scaling-aws-control-tower-controls-using-amazon-bedrock-agents/blob/main/get-enable-control-status/lambda_function.py">repository</a> and click deploy.</li> 
</ul> 
<h2><strong>Step 4: Setting up IAM permissions:</strong></h2> 
<p>As a last step, we need to set up three IAM roles: two in the GenAI Tooling Account and one in the AWS Management Account. These will give access to the AWS Lambda functions to call AWS Control Tower Control APIs in the AWS Management account.</p> 
<h3><strong>&nbsp;</strong><strong>In GenAI Tooling Account:</strong></h3> 
<ul> 
 <li>Navigate to the <a href="https://console.aws.amazon.com/iam">Amazon Identity and Access Management</a>, and <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html#access_policies_create-json-editor">create an IAM policy</a> using the policy code provided <a href="https://github.com/aws-samples/scaling-aws-control-tower-controls-using-amazon-bedrock-agents/blob/main/lambda-iam-role/tooling-account/iam-policy.json">here</a>.</li> 
 <li><a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-custom.html#roles-creatingrole-custom-trust-policy-console">Create an IAM role using custom trust policy</a>, and attach the permission policy created in the previous step. You can refer to the custom trust policy <a href="https://github.com/aws-samples/scaling-aws-control-tower-controls-using-amazon-bedrock-agents/blob/main/lambda-iam-role/tooling-account/trust-policy.json">here</a>.</li> 
 <li>Navigate to&nbsp;<a href="https://console.aws.amazon.com/lambda">AWS Lambda console&nbsp;</a> and search for <em>List enabled control </em>Lambda function. <a href="https://docs.aws.amazon.com/lambda/latest/dg/permissions-executionrole-update.html#update-execution-role">Update the function’s execution role</a> with the role created above. The same role needs to be updated for <em>Enable/disable control, Bulk enable/disable controls </em>and <em>Get enabled control status </em>Lambda functions.</li> 
 <li>Follow steps (i) and (ii) to create another role and update the <em>Find all relevant controls </em>Lambda function. You can refer to the IAM policy and custom trust policy <a href="https://github.com/aws-samples/scaling-aws-control-tower-controls-using-amazon-bedrock-agents/tree/main/find-all-relevant-controls">here</a>.</li> 
</ul> 
<h3><strong>In the AWS Management Account:</strong></h3> 
<ul> 
 <li>Follow steps (i) and (ii) to create another role that grants cross-account permissions for Lambda functions in the GenAI Tooling Account to manage AWS Control Tower controls in the AWS Management Account. You can refer to the IAM policy and custom trust policy <a href="https://github.com/aws-samples/scaling-aws-control-tower-controls-using-amazon-bedrock-agents/tree/main/lambda-iam-role/management-account">here</a>.</li> 
</ul> 
<p><strong>Note: </strong>Update account IDs and cross-account role names to ensure they are current and accurate.</p> 
<h2><strong>Example Interactions</strong></h2> 
<p>Now that the solution has been deployed, let’s take a look at example interactions with Amazon Bedrock Agent with AWS Control Tower Controls.</p> 
<p><strong>Use-case 1</strong>: A user initiates a session with the Amazon Bedrock Agent intending to bulk enable controls, as shown in Figure 2, however they don’t know the control identifiers. Then, the user provides a description, such as, “Public IP addresses” to locate the relevant control identifier. The Amazon Bedrock Agent then invokes the <em>Find all relevant controls</em> action group, which lists all controls related to the provided description.</p> 
<p><a href="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2024/10/20/blog-chat-1.png"><img alt="Example interaction with Amazon Bedrock Agent and AWS Control Tower Control APIs" class="aligncenter wp-image-57260 size-full" height="932" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2024/10/20/blog-chat-1.png" width="1698" /></a></p> 
<p style="text-align: center;"><em>Figure 2. Amazon Bedrock Agent lists the relevant control identifiers for user provided description</em></p> 
<p>From the provided list of relevant control identifiers, the user specifies the controls that need to be enabled. The Amazon Bedrock Agent gathers the required parameters such as Region and OUs from the user and triggers the <em>Bulk enable/disable controls</em> action group to enable the controls for “Autoscaling Launch Configuration Public IP Disabled” and “EC2 Instance No Public IP” as shown in Figure 3.</p> 
<p><a href="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2024/10/21/blog-chat-2-1.png"><img alt="Example interaction with Amazon Bedrock Agent and AWS Control Tower APIs" class="aligncenter wp-image-57289 size-full" height="938" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2024/10/21/blog-chat-2-1.png" width="1680" /></a></p> 
<p style="text-align: center;"><em>Figure 3. Amazon Bedrock Agent confirms that user provided controls have been enabled</em></p> 
<p style="text-align: left;"><strong>Use-case 2: </strong>To obtain detailed information and the current status of a specific enabled control, the user needs to pass the enabled controls <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/reference-arns.html">Amazon Resource Names</a> (ARN) to the Amazon Bedrock agent. The agent triggers the <em>Get enabled control status</em> action group which uses the <a href="https://aws.amazon.com/about-aws/whats-new/2024/08/aws-control-tower-new-descriptive-control-apis/">GetControl</a> API to retrieve the details. An example is shown in Figure 4 below:</p> 
<p style="text-align: center;"><a href="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2024/10/28/use-case-get-control.png"><img alt="Use case to get details about specific enabled control present" class="aligncenter wp-image-57770" height="455" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2024/10/28/use-case-get-control.png" width="600" /></a><em>Figure 4. Example use case to get details about specific enabled control</em></p> 
<h2>Clean up</h2> 
<p>The services used in this demonstration can incur costs. Complete the following steps to clean up your resources:</p> 
<ol> 
 <li>Delete the Lambda functions if they’re no longer required.</li> 
 <li>Delete action groups and Amazon Bedrock agent that were created.</li> 
 <li>Empty and delete the S3 bucket used for storing files.</li> 
 <li>Delete the Amazon Bedrock knowledge base&nbsp;Bedrock if it’s no longer needed.</li> 
</ol> 
<h2><strong>Conclusion</strong></h2> 
<p>By leveraging Amazon Bedrock Agents to automate AWS Control Tower control management, we can streamline the AWS Control Tower control operations. The solution eliminates the need for control identifier lookups and improves the experience of interacting with AWS Control Tower control APIs. Users can now specify their requirements through an Amazon Bedrock Agent chat interface, and the agent handles the rest, ensuring accurate and efficient control management. This approach not only saves time, but also enhances the overall user experience, making AWS Control Tower control operations more accessible and user-friendly.</p>
