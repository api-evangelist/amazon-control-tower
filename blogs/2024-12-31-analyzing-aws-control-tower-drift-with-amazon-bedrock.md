---
title: "Analyzing AWS Control Tower Drift with Amazon Bedrock"
url: "https://aws.amazon.com/blogs/mt/analyzing-aws-control-tower-drift-with-amazon-bedrock/"
date: "Tue, 31 Dec 2024 19:37:30 +0000"
author: "Craig Edwards"
feed_url: "https://aws.amazon.com/blogs/mt/category/management-tools/aws-control-tower/feed/"
---
<h2>Introduction</h2> 
<p>In order to enforce best practices for governance and compliance across AWS accounts in a centralized way, <a href="https://aws.amazon.com/controltower/">AWS Control Tower</a> is an easy place to start.&nbsp; However, ensuring continuous compliance requires regular <a href="https://docs.aws.amazon.com/controltower/latest/userguide/drift.html">drift detection</a> and remediation, which Control Tower facilitates by providing a mechanism to detect drift and publish notifications to <a href="https://aws.amazon.com/sns/">Amazon Simple Notification Service</a> (SNS) topics.&nbsp; AWS Control Tower Drift occurs when configuration changes in your AWS environment deviate from the baseline set by AWS Control Tower. Drift can occur due to manual changes outside of Control Tower, unintended configuration changes, or updates to AWS services and features.</p> 
<p>Unmanaged configuration changes, or “drift”, can undermine the effectiveness of centralized governance frameworks like AWS Control Tower. Avoiding and quickly resolving drift helps ensure that your cloud environment remains compliant with organizational policies and industry regulations, thereby mitigating risks. When drift occurs, it can potentially disrupt Control Tower’s ability to manage your Organizational Units (OUs) and therefore the accounts used to apply controls, monitor compliance, and enforce governance throughout your organization. Being able to quickly analyze and remediate drift enhances your organization’s compliance operations and overall security posture.</p> 
<p>Understanding the various types of drift, determining their root causes, and remediating the issues add additional operational overhead and consume time that could be dedicated to innovating or modernizing your infrastructure or applications.</p> 
<p>In this blog post we will show you how <a href="https://aws.amazon.com/bedrock/">Amazon Bedrock</a> can be used to streamline the process.&nbsp; Amazon Bedrock is a fully managed AI service that can help you analyze and understand the complex data associated with AWS Control Tower drift.&nbsp; For example, Amazon Bedrock can automatically categorize the different types of drift detected, identify the root cause, and even suggest remediation steps. By leveraging Amazon Bedrock’s natural language processing and machine learning capabilities, you can quickly gain insights into your drift data and take appropriate action to maintain compliance without adding significant operational overhead.</p> 
<p>In the next section, we’ll demonstrate how you can use Amazon Bedrock to analyze AWS Control Tower drift and outline the steps for remediation, helping ensure your AWS environment maintains consistent governance.</p> 
<h2>Overview of solution</h2> 
<p>This workflow diagram illustrates an AWS cloud native solution for detecting and responding to configuration drifts. Here’s an explanation of the process:</p> 
<ol> 
 <li>The workflow starts in the Management Account, where Control Tower detects a configuration drift. Once detected, the process moves to the account designated for <a href="https://docs.aws.amazon.com/controltower/latest/userguide/logging-and-monitoring.html">centralized logging</a> (often referred to as the <a href="https://docs.aws.amazon.com/controltower/latest/userguide/accounts.html#audit-account">Audit account</a> in a typical Control Tower setup, but this may vary based on your organization’s structure). 
  <ol> 
   <li><a href="https://aws.amazon.com/cloudtrail/">AWS CloudTrail</a> records the drift event as part of its management events. Data events can be configured in order to capture more detailed information from relevant resources.</li> 
   <li>This CloudTrail event triggers an Amazon Simple Notification Service (<a href="https://docs.aws.amazon.com/controltower/latest/userguide/sns.html">SNS</a>) Topic.</li> 
  </ol> </li> 
 <li>The SNS Topic then activates and sends an email notification as part of AWS Control Tower.</li> 
 <li>The Knowledge Base feeds information to an <a href="https://docs.aws.amazon.com/bedrock/latest/userguide/agents.html">Amazon Bedrock Agent</a>.</li> 
</ol> 
<p>The Amazon Bedrock Agent then analyzes the drift information and provides insights into the root cause and potential impact of the detected drift. This setup allows for automated detection, notification, and analysis of configuration drifts in the AWS environment. It leverages various AWS services to create a comprehensive monitoring and response system, helping maintain desired configurations across the cloud infrastructure.</p> 
<p>While this solution primarily focuses on drift detection and analysis, it lays the groundwork for potential future automation of remediation steps, pending careful consideration and implementation of appropriate safeguards.</p> 
<div class="wp-caption aligncenter" id="attachment_58845" style="width: 819px;">
 <img alt="Architecture diagram depicting solution flow" class="wp-image-58845 size-full" height="510" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2024/12/31/GenAI-CT-Drift-Solution.png" width="809" />
 <p class="wp-caption-text" id="caption-attachment-58845">Figure 1: Architecture diagram showing Amazon Bedrock analyzing AWS Control Tower Drift</p>
</div> 
<h2></h2> 
<h2>Using Amazon Bedrock Agents to Analyze and Resolve Drift</h2> 
<p>To demonstrate Amazon Bedrock’s capabilities, we can intentionally create drift in our AWS Control Tower environment. A common method is modifying a resource outside of AWS Control Tower guardrails.</p> 
<p>When the drift notification is received, you can leverage Bedrock to gain deeper insights into the issue. Bedrock can analyze the drift notification and provide meaningful information about the root cause of the drift. For example, if the notification references drift due to <code>ACCOUNT_MOVED_BETWEEN_OUS</code>.&nbsp; Amazon Bedrock can explain which Organizational Unit (OU) the account was moved to and from, and explains why the change caused the drift.</p> 
<h2>Key Benefits of Using Amazon Bedrock for Cloud Governance</h2> 
<ul> 
 <li><strong>Proactive Monitoring and Alerts:</strong> Amazon Bedrock continuously monitors your AWS environment, providing real-time alerts when drifts are detected, ensuring immediate attention and quick action.&nbsp; To leverage Bedrock for proactive monitoring and alerting of AWS Control Tower drift, you would need to set up Bedrock agents to continuously ingest and analyze the drift notifications from your Control Tower SNS topics. This could be done as a scheduled job or triggered by new drift events.</li> 
 <li><strong>Root Cause Analysis:</strong> By identifying the underlying causes of configuration drifts, Bedrock helps in preventing future occurrences, thereby enhancing the stability and governance of your cloud infrastructure.</li> 
 <li><strong>Step by Step Remediation:</strong> While Bedrock can analyze the details of the detected drift and provide insights into the root cause, the process of actually remediating the drift would likely still require some manual effort. Bedrock does not currently have the ability to automatically remediate drift issues in AWS Control Tower.&nbsp; However, Bedrock can provide valuable information to streamline the remediation process. By analyzing the drift notification data and correlating it with CloudTrail logs, Bedrock can identify the specific actions or configuration changes that led to the drift. This information can then be used to quickly determine the appropriate remediation steps.</li> 
 <li><strong>Automated Remediation:</strong> Bedrock can help generate code that can be used to automatically remediate certain types of drifts, reducing the need for manual intervention and allowing your team to focus on more strategic tasks.</li> 
</ul> 
<h2>Prerequisites</h2> 
<p>Before getting started, ensure you have:</p> 
<ul> 
 <li>An AWS account with <a href="https://docs.aws.amazon.com/controltower/latest/userguide/setting-up.html">AWS Control Tower set up</a>.</li> 
 <li>Access to the <a href="https://docs.aws.amazon.com/controltower/latest/userguide/accounts.html">AWS Control Tower Management account</a>.</li> 
 <li>Access to the designated <a href="https://docs.aws.amazon.com/controltower/latest/userguide/logging-using-cloudtrail.html">logging account</a> (often referred to as the Audit account in a typical Control Tower setup).</li> 
 <li>Access to the AWS Control Tower SNS topic (<code>aws-controltower-AggregateSecurityNotifications</code>)<br /> where drift notifications are published.</li> 
</ul> 
<div class="wp-caption aligncenter" id="attachment_58810" style="width: 1034px;">
 <img alt="AWS Control Tower SNS Topic" class="wp-image-58810 size-large" height="211" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2024/12/26/SNS-Topic-AggregateSecurity-Notification-1024x211.png" width="1024" />
 <p class="wp-caption-text" id="caption-attachment-58810">Figure 2: AWS Control Tower SNS notifications</p>
</div> 
<ul> 
 <li>Permissions to read and process CloudTrail logs to analyze the root causes of drift. This includes the following <a href="https://docs.aws.amazon.com/controltower/latest/userguide/auth-access.html">IAM permissions</a>:</li> 
</ul> 
<ul> 
 <li> 
  <ul> 
   <li>Management events</li> 
   <li>Data events for specific resources relevant to Control Tower drift</li> 
   <li>Read access to CloudTrail Lake if implemented</li> 
   <li>Permissions to access and configure Amazon Bedrock, including creating agents and processing data.</li> 
   <li>Access to Anthropic Foundational Models</li> 
   <li>SNS permissions for subscribing to and publishing notifications</li> 
   <li><a href="https://aws.amazon.com/cloudwatch/">Amazon CloudWatch</a> permissions for metric data retrieval</li> 
  </ul> </li> 
</ul> 
<p>It’s crucial to implement least-privilege access principles, granting only the necessary permissions for the Bedrock agents to perform their required tasks.&nbsp; For a comprehensive list of required permissions and best practices, please refer to the official AWS documentation:</p> 
<ul> 
 <li><a href="https://docs.aws.amazon.com/controltower/latest/userguide/permissions.html">AWS Control Tower Permissions</a></li> 
 <li><a href="https://docs.aws.amazon.com/bedrock/latest/userguide/security.html">Amazon Bedrock Security</a></li> 
</ul> 
<h2>Agent Creation Workflow</h2> 
<p>To get started with using Amazon Bedrock to analyze and remediate AWS Control Tower drift, we’ll need to first create custom agents within the Bedrock service. These agents will be configured with specific instructions and capabilities to handle different aspects of the drift analysis and remediation process.</p> 
<p>Here’s the step-by-step process for creating the Bedrock agents:</p> 
<h2>Bedrock Agent : Analyze root cause</h2> 
<ol> 
 <li>Log in to the <strong>Amazon Bedrock</strong> console and select <strong>Agents</strong> in the left pane navigation.</li> 
 <li>Select <strong>Create Agent</strong>.</li> 
</ol> 
<p>Provide the following details:</p> 
<ul> 
 <li><strong>Agent name:</strong> drift-root-cause-agent</li> 
 <li><strong>Agent description:</strong>&nbsp; This agent helps you analyze the root cause of drift and it will give you the steps for remediation.</li> 
 <li><strong>Create New Role:</strong> Add the permissions to the role that were mentioned in the prerequisites.</li> 
 <li><strong>Select model:</strong> Anthropic – Claude Versions may vary (For the example testing we used the model <strong>Claude 3.5 Sonnet</strong>).</li> 
 <li><strong>Instructions for the Agent:</strong> Identify root cause of Control Tower drift and associated CloudTrail event.</li> 
 <li>Select <strong>Save and Exit</strong>, then <strong>Prepare.</strong></li> 
</ul> 
<div class="wp-caption aligncenter" id="attachment_58838" style="width: 1269px;">
 <img alt="Amazon Bedrock Agent configuration details." class="wp-image-58838 size-full" height="648" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2024/12/31/Bedrock-Agent-Creation-Updated-1.png" width="1259" />
 <p class="wp-caption-text" id="caption-attachment-58838">Figure 3: Amazon Bedrock Agent Configuration Details.</p>
</div> 
<h2></h2> 
<h2>Creating a Knowledge Base</h2> 
<p>An <a href="https://docs.aws.amazon.com/bedrock/latest/userguide/kb-how-it-works.html">Amazon Bedrock Knowledge Base</a> is a foundational repository of information and data used by AI models to understand and respond to queries. It serves as the core knowledge from which we draw insights, facts, and contextual understanding to provide accurate and relevant responses to users’ questions and requests.</p> 
<p>For the agent to understand the queries, you should input relevant <a href="https://docs.aws.amazon.com/whitepapers/latest/organizing-your-aws-environment/appendix-e-establish-multi-account.html">AWS Control Tower documentation</a> into an <a href="https://aws.amazon.com/s3/">Amazon Simple Storage Service</a> (Amazon S3) bucket in the audit account. This documentation can be found in the official <a href="https://docs.aws.amazon.com/controltower/latest/userguide/what-is-control-tower.html">AWS Control Tower User Guide</a>.</p> 
<p>You can download relevant sections of this guide and <a href="https://docs.aws.amazon.com/bedrock/latest/userguide/s3-data-source-connector.html">upload them to your S3 bucket</a>. Additionally, uploading a sample Control Tower Drift JSON file will allow you to run queries based on the instructions for the agent.</p> 
<p>Make sure to follow AWS best practices for securing <a href="https://docs.aws.amazon.com/AmazonS3/latest/userguide/GetStartedWithS3.html">S3</a> buckets when storing this information.</p> 
<div class="wp-caption aligncenter" id="attachment_58812" style="width: 1859px;">
 <img alt="Knowledge base for the agent to understand AWS Control Tower. " class="size-full wp-image-58812" height="874" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2024/12/26/Screenshot-2024-11-25-at-10.19.25-AM.png" width="1849" />
 <p class="wp-caption-text" id="caption-attachment-58812">Figure 4: Attach the new Knowledge Base to your drift analysis agent.</p>
</div> 
<h2>Test the Agent</h2> 
<p><a href="https://docs.aws.amazon.com/bedrock/latest/userguide/prompt-engineering-guidelines.html">Prompt Engineering</a> refers to the practice of optimizing textual input to a Large Language Model (LLM) to obtain desired responses. Below, you can see the prompts we used for the Bedrock Agent testing.</p> 
<p><strong>Prompts:</strong></p> 
<ul> 
 <li><strong>Example Prompt:</strong> “Tell me about an event of Control Tower Drift that has occurred in my managed account”</li> 
 <li><strong>Example prompt:</strong> “Analyze drift for Account ID: xxxxxxxxxxxx in Organization ID: o-exampleorgid” 
  <ul> 
   <li>Remediation is given.</li> 
  </ul> </li> 
 <li><strong>Example prompt:</strong> “Can you elaborate more on the remediation”</li> 
</ul> 
<div class="wp-caption aligncenter" id="attachment_58839" style="width: 3752px;">
 <img alt="Prompting the bedrock agent to provide remedation steps" class="wp-image-58839 size-full" height="1778" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2024/12/31/Control-Tower-Drift-Bedrock-Agent1-1.png" width="3742" />
 <p class="wp-caption-text" id="caption-attachment-58839">Figure 5: Example prompts to the Amazon Bedrock agent for remediation steps.</p>
</div> 
<p>The agent should respond to the prompts and consistently provide steps to remediate the drift to the AWS Control Tower environment as shown in the example above.</p> 
<p>Our Amazon Bedrock agent should return:</p> 
<ol> 
 <li><strong>Accurate identification of the drift event:</strong> The agent should correctly recognize and describe the specific Control Tower drift that occurred.</li> 
 <li><strong>Detailed root cause analysis:</strong> It should provide a clear explanation of why the drift happened, identifying the actions or changes that led to the deviation from the Control Tower baseline.</li> 
 <li><strong>Potential impact assessment:</strong> The agent should outline possible consequences of the drift on your AWS environment’s compliance and security posture.</li> 
 <li><strong>Actionable remediation steps:</strong> It should offer specific, step-by-step guidance on how to resolve the drift and bring the environment back into compliance with Control Tower standards.</li> 
 <li><strong>Relevant AWS documentation references:</strong> The agent should provide links to official AWS documentation for further information on the drift type and remediation processes.</li> 
 <li><strong>Consistency across different drift scenarios:</strong> We want the agent to maintain this level of detailed analysis and guidance for various types of Control Tower drift, not just for one specific example.</li> 
</ol> 
<h2>Clean up</h2> 
<p>To avoid incurring future charges and maintain a clean environment, follow these steps to delete the resources created during this exercise:</p> 
<ol> 
 <li><a href="https://docs.aws.amazon.com/bedrock/latest/userguide/agents-delete.html">Delete Bedrock agents</a> and associated<a href="https://docs.aws.amazon.com/bedrock/latest/userguide/kb-delete.html"> knowledge base</a>.</li> 
 <li><a href="https://docs.aws.amazon.com/AmazonS3/latest/userguide/delete-bucket.html">Delete any S3 buckets</a> created to store documentation or drift data.</li> 
 <li><a href="https://docs.aws.amazon.com/sns/latest/dg/sns-delete-subscription-topic.html">Remove SNS topics and subscriptions</a> that are no longer needed.</li> 
 <li>Review and <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_delete.html">revoke any IAM roles or policies</a> that are no longer needed.</li> 
</ol> 
<p>Remember to review your AWS account for any other resources that might have been created during this project and remove them if they’re no longer needed. Always double-check before deleting resources to ensure you’re not removing anything critical to your operations.</p> 
<h2>Conclusion</h2> 
<p>Using Amazon Bedrock to analyze AWS Control Tower drift provides a powerful way to maintain compliance and governance in your AWS environment. By analyzing the root cause of the drift and remediation steps, you can ensure that your AWS resources remain aligned with best practices and ensures Control Tower can maintain governance over the Landing Zone.</p> 
<p>For more information on Amazon Bedrock and AWS Control Tower, see <a href="https://docs.aws.amazon.com/">AWS documentation</a> and search for the appropriate service.</p> 
<h2>References</h2> 
<ul> 
 <li><a href="https://docs.aws.amazon.com/controltower/latest/userguide/what-is-control-tower.html">AWS Control Tower Documentation</a></li> 
 <li><a href="https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html">Amazon Bedrock Documentation</a></li> 
</ul> 
<h2>Next Steps</h2> 
<p>In this blog post, we’ve demonstrated how Amazon Bedrock can be used to analyze and gain insights into AWS Control Tower drift, helping you maintain compliance and security in your multi-account AWS environment.</p> 
<p>To further enhance your drift detection and remediation capabilities, we recommend the following next steps:</p> 
<ol> 
 <li><strong>Explore more features of Amazon Bedrock:</strong> Investigate additional Bedrock capabilities, such as customizable data processing pipelines and advanced analytics, to tailor the solution to your specific needs.</li> 
 <li><strong>Implement continuous monitoring:</strong> Set up a robust Bedrock-powered monitoring system to proactively manage compliance in your AWS environment, ensuring you can quickly identify and resolve drift issues.</li> 
 <li><strong>Stay updated with AWS best practices:</strong> Regularly review AWS guidance and updates to optimize your cloud governance strategy and ensure your Control Tower and Bedrock configurations align with the latest recommended practices.</li> 
</ol> 
<p>Ready to get started? Schedule a consultation with our <a href="https://aws.amazon.com/contact-us/?nc1=f_m">AWS experts</a> to learn how Amazon Bedrock can be integrated into your AWS Control Tower drift management process. Our team can help you design and implement a custom solution that meets your specific requirements and helps you maintain a secure, compliant, and well-governed cloud environment.</p> 
<footer> 
 <div class="blog-author-box"> 
  <div class="blog-author-image">
   <img alt="Abraham Musa author photo" class="wp-image-11636 alignleft" height="160" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2023/11/26/abeMusa.png" width="120" />
  </div> 
  <h3 class="lb-h4">Abraham Musa</h3> 
  <p style="text-align: left;">Abraham is a Cloud Operations Specialist Solutions Architect with the Cloud Foundations team at AWS based out of New York. He specializes in AWS Control Tower, AWS Organizations, AWS Service Catalog, and AWS Config. Abraham is a United States Army Veteran and enjoys traveling.</p> 
 </div> 
</footer>
