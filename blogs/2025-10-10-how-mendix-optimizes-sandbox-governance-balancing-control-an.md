---
title: "How Mendix optimizes sandbox governance, balancing control and productivity"
url: "https://aws.amazon.com/blogs/mt/how-mendix-optimizes-sandbox-governance-balancing-control-and-productivity/"
date: "Fri, 10 Oct 2025 15:21:43 +0000"
author: "Sachin Purohit"
feed_url: "https://aws.amazon.com/blogs/mt/category/management-tools/aws-control-tower/feed/"
---
<p>In today’s cloud-driven landscape, development sandboxes have become enablers of innovation, offering safe environments for experimentation and testing. However, as organizations scale, these sandbox environments often grow increasingly complex and difficult to manage. Unchecked, this complexity can lead to escalating costs from abandoned resources, increased security risks, and diminished productivity—undermining the very benefits sandboxes are intended to provide.</p> 
<p>This was the challenge faced by <a href="https://www.mendix.com/">Mendix</a>, an AWS partner, which provides a low-code application development platform to their users. By implementing a sandbox governance strategy across four key pillars, Mendix not only realized substantial cost savings, but also enhanced developer productivity and agility. Their journey from sandbox sprawl to operational excellence offers valuable insights for any organization navigating similar issues.</p> 
<p>In this blog post, we will delve into the strategies and best practices that enabled Mendix to regain control over their sandbox environments. We will explore how governance can balance developer autonomy with operational discipline, ensuring resources are managed, costs are contained, and innovation continues to thrive.</p> 
<h1>Overview of solution</h1> 
<p>The four critical pillars that formed the foundation of Mendix’s sandbox governance are explained below</p> 
<ol> 
 <li> <h2>Automatic Sandbox Account Provisioning:</h2> <p>Through automated account provisioning, Mendix created standardized sandbox environments in minutes rather than days. The automated mechanism ensures that each sandbox account is created with pre-configured security guardrails, platform resources and IAM policies aligned with organizational standards.</p> <p>By implementing automated account vending integrated with <a href="https://docs.aws.amazon.com/controltower/latest/userguide/aft-overview.html">AWS Control Tower Account Factory for Terraform (AFT)</a> and <a href="https://aws.amazon.com/controltower/">AWS Control Tower</a>, Mendix reduced their sandbox account provisioning time from days to just 15 minutes, while ensuring consistent compliance and security controls across all environments.</p> <p><img alt="Sandbox account provisioning Automation" class="aligncenter wp-image-63013 size-full" height="701" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/10/07/Figure-1-Automatic-SandboxAccountProvisioning.png" width="911" /></p> <p style="text-align: center;">Figure 1 Sandbox account provisioning Automation</p> </li> 
</ol> 
<h3 style="text-align: left; padding-left: 40px;"><strong>&nbsp;Initial Request Processing:</strong></h3> 
<ol> 
 <li> 
  <ol> 
   <li> 
    <ol> 
     <li>User fills the form and submits a ticket in the ticketing tool to request a sandbox account.</li> 
     <li>Topdesk <a href="https://docs.topdesk.com/VA2023R2/en/setting-up-events-and-actions.html">events and actions</a> triggers a HTTP post requests to an <a href="https://aws.amazon.com/api-gateway/">Amazon API Gateway</a></li> 
     <li>The API Gateway endpoint, with the help of a <a href="https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-use-lambda-authorizer.html">custom authorizer</a> and an API Gateway <a href="https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-resource-policies.html">resource policy</a> checks the authenticity of the request. If the request is legitimate, the authorizer assigns privilege to <a href="https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-call-api.html">invoke</a> the API endpoint.</li> 
     <li>The API endpoint triggers the provisioning <a href="https://aws.amazon.com/pm/lambda/?gclid=EAIaIQobChMIpOqGmdrwjAMV7IhQBh01igeKEAAYASAAEgLd-fD_BwE&amp;trk=73f686c8-9606-40ad-852f-7b2bcafa68fe&amp;sc_channel=ps&amp;ef_id=EAIaIQobChMIpOqGmdrwjAMV7IhQBh01igeKEAAYASAAEgLd-fD_BwE:G:s&amp;s_kwcid=AL!4422!3!651212652666!e!!g!!lambda!909122559!45462427876&amp;gbraid=0AAAAADjHtp8FmjRnCLyweZ6ZX2DXHrZCK">AWS Lambda function</a> with payloads containing ticket id, requester info and the details required by AFT to process account vending request.</li> 
     <li>The provisioning Lambda function processes the event, validates the input provided, prepares the data in the acceptable format and pushes the code to Mendix’s Gitlab repository to create the account.</li> 
    </ol> </li> 
  </ol> </li> 
</ol> 
<h3 style="padding-left: 40px;"><strong>Account Creation Phase:</strong></h3> 
<ol> 
 <li> 
  <ol> 
   <li> 
    <ol> 
     <li>Gitlab then triggers the AFT engine which processes the account request further.</li> 
     <li>AFT creates the sandbox account with the help of <a href="https://docs.aws.amazon.com/controltower/latest/userguide/account-factory.html">AWS Control Tower Account Factory</a>.</li> 
     <li>Upon successful creation of the sandbox account, AWS Control Tower triggers lifecycle events which in turn triggers AFT <a href="https://docs.aws.amazon.com/controltower/latest/userguide/aft-account-customization-options.html">account customizations</a>.</li> 
    </ol> </li> 
  </ol> </li> 
</ol> 
<h3 style="padding-left: 40px;"><strong>Account Customization and Completion:</strong></h3> 
<ol> 
 <li> 
  <ol> 
   <li> 
    <ol> 
     <li>Below actions are taken by AFT using customization framework: 
      <ol type="a"> 
       <li>provisions access to the sandbox account using IAM Identity Center.</li> 
       <li>Baselines the sandbox account with the required policies and resources using AFT account customization.</li> 
      </ol> </li> 
     <li>AFT creates an <a href="https://aws.amazon.com/codepipeline/">AWS CodePipeline</a> for applying customization, and this event is captured by an <a href="https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-rules.html">Amazon EventBridge rule</a> which then triggers the post provisioning Lambda function.</li> 
     <li>The post provisioning Lambda function fetches necessary metadata (ticket id, requester email) stored by AFT, resolves the ticket and notifies requester about the details of the Sandbox account and login instructions.</li> 
    </ol> </li> 
  </ol> </li> 
</ol> 
<h2>Budget Monitoring:</h2> 
<p>Cost visibility and control are paramount in managing sandbox environments at scale. Implementing proactive budget monitoring helped Mendix maintain fiscal responsibility without stifling innovation. This pillar involves setting up automated cost thresholds, real-time spending alerts, and granular resource tracking at both the account and project level.</p> 
<p>Mendix implemented dynamic budget controls that automatically notify the account owner when they reach soft limit (50%) set for their allocated budget, the sandbox account owner can then take necessary action to stay within the budget. If they breach the hard limit (close to 100%) of the set budget, the account is then isolated to stop users from creating additional resources.</p> 
<p><img alt="Budget Monitoring" class="alignnone wp-image-63040 size-full" height="481" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/10/08/Figure-2-BudgetMonitoring-1.png" width="1121" /></p> 
<p style="text-align: center;">Figure 2 Sandbox Account Budget Monitoring Solution</p> 
<p>Technical workflow utilizes AWS services as mentioned below:</p> 
<ol> 
 <li> 
  <ol> 
   <li><a href="https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html">AWS Budgets</a> will monitor the Cost associated with the individual Sandbox account.</li> 
   <li>If the set threshold of soft limit/Hard limit is breached, it will send the event to the Lambda function in the AFT management account via <a href="https://docs.aws.amazon.com/sns/latest/dg/welcome.html">Amazon Simple Notification Service</a>.</li> 
   <li>The Lambda function will parse the event and perform following action: 
    <ol type="a"> 
     <li>If the Actual usage amount exceeds the set soft limit, it sends a notification to the sandbox account owner.</li> 
     <li>If the Actual usage amount reaches the set hard limit, it attaches a <a href="https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html">Service Control Policy (SCP)</a> to the Sandbox account which denies users the ability to perform any API activities on the Account and sends a notification to the Sandbox account owner and the Mendix IT support team.</li> 
     <li>The Sandbox account owners can then work with the IT support team to delete some resources to get the budget under the required threshold and enable the resumption of normal usage for their sandbox account.</li> 
    </ol> </li> 
  </ol> </li> 
</ol> 
<h2>Resource Recycling:</h2> 
<p>Automated resource recycling represents a critical pillar of sandbox governance. This mechanism allows Mendix to systematically identify and decommission unused resources on a scheduled basis, ensuring development assets don’t persist beyond their intended lifecycle. Their resource recycling system automatically decommissions resources every quarter, while preserving platform created resources. This approach led to a 60% reduction in infrastructure cost without disrupting developer workflows.</p> 
<p><img alt="ResourceRecycling" class="aligncenter wp-image-63015 size-large" height="626" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/10/07/Figure-3-ResourceRecycling-1024x626.png" width="1024" /></p> 
<p style="text-align: center;">Figure-3 Resource Recycling</p> 
<ol> 
 <li> 
  <ol> 
   <li>As depicted in Figure-3, For each of the vended Sandbox accounts there is an <a href="https://docs.aws.amazon.com/scheduler/latest/UserGuide/what-is-scheduler.html">EventBridge scheduler</a> created in the AFT Management account. On every schedule (1st of every quarter), EventBridge Scheduler invokes the <a href="https://aws.amazon.com/step-functions/">AWS Step Functions state machine</a> with necessary inputs like Account Id and allowlisted regions the account is permitted to operate in.</li> 
   <li>The Step Functions state machine will navigate to each region in the target account by assuming an IAM role and perform the following actions: 
    <ol type="a"> 
     <li>A Lambda function checks if the account belongs to the Sandbox <a href="https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_ous.html">Organizational Unit</a> (OU) to ensure a safety net to prevent recycle operation in any non-sandbox account. 
      <ol type="i"> 
       <li>If the account is part of Sandbox OU, Create an IAM Role which the <a href="https://docs.aws.amazon.com/codebuild/latest/userguide/welcome.html">AWS CodeBuild</a> job can assume to recycle the in-scope resources.</li> 
       <li>If the account doesn’t belong to the Sandbox OU, the automation exits without executing any further steps.</li> 
      </ol> </li> 
     <li>A Step Functions <a href="https://docs.aws.amazon.com/step-functions/latest/dg/state-map.html">map state</a> navigates to regions provided as input, in parallel for an account using CodeBuild remote invocation jobs to perform the following: 
      <ol type="i"> 
       <li>Download the recycle configuration file from the central <a href="https://aws.amazon.com/pm/serv-s3/?gclid=EAIaIQobChMIgbufi93wjAMVt4BQBh3KhzccEAAYASAAEgJUQ_D_BwE&amp;trk=fecf68c9-3874-4ae2-a7ed-72b6d19c8034&amp;sc_channel=ps&amp;ef_id=EAIaIQobChMIgbufi93wjAMVt4BQBh3KhzccEAAYASAAEgJUQ_D_BwE:G:s&amp;s_kwcid=AL!4422!3!536452728638!e!!g!!amazon%20s3!11204620052!112938567994&amp;gbraid=0AAAAADjHtp9SdqWkoGgi5bn1v8uBZWuXt">Amazon S3</a> bucket which specifies in-scope and out-of-scope resources for recycling.</li> 
       <li>Start terminating the in-scope resources and sends notifications to the account owners about the activity.</li> 
      </ol> </li> 
     <li>A Lambda function cleans up the IAM Role created in the earlier step to ensure the high privileged role only exists during the lifetime of the automation.</li> 
    </ol> </li> 
   <li>Another EventBridge schedule runs multiple times during the quarter to send regular notifications to the Sandbox account owners about the quarterly resource recycling event.</li> 
  </ol> </li> 
</ol> 
<h2>Controls:</h2> 
<p>Security controls are standardized safeguards, including policies, procedures, and technical configurations, that organizations implement to protect their IT resources and ensure compliance. Within AWS sandbox environments, these controls operate by automatically restricting and monitoring user actions. This allows developers to experiment freely while preventing activities that could compromise security or exceed organizational boundaries</p> 
<p>These controls fall into two distinct categories as explained below</p> 
<h3>Preventive Controls:</h3> 
<p>Mendix team has implemented preventive Controls in the form of AWS Organizations <a href="https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html">service control policies (SCP)</a> to prevent non-compliance in their sandbox environment like restricting sandbox account to operate only in the approved regions, protection of platform created resources from modification, deny leaving Organizations etc. These guardrails proactively block actions that violate the Organizations policies.</p> 
<h3>Detective Controls:</h3> 
<p>Mendix team leverages AWS Config to detect configuration changes and policy violations in their Sandbox environment. Additionally, they also use a third-party security monitoring tool, creating a comprehensive system that continuously assesses resource configurations and potential vulnerabilities. When non-compliant states are detected, the system automatically notifies relevant stakeholders, including the Security team and Cloud Center of Excellence. This approach ensures that any deviations from established security baselines or best practices are promptly identified and addressed</p> 
<h1>Conclusion</h1> 
<p>In this blog post, we’ve explored how Mendix transformed their cloud sandbox governance through a comprehensive four-pillar strategy: automated account provisioning, budget monitoring, resource recycling, and security controls. This approach reduced account provisioning time from days to 15 minutes, while cutting sandbox-related cloud costs by 60%. The implementation created a sustainable and efficient sandbox environment that enables Mendix to maintain strict cost control while accelerating innovation cycles.</p> 
<p>To begin your sandbox governance journey, start by exploring AWS Control Tower’s <a href="https://docs.aws.amazon.com/controltower/latest/userguide/account-factory.html">Account Factory</a> and <a href="https://docs.aws.amazon.com/controltower/latest/userguide/taf-account-provisioning.html">AFT</a> for automated account provisioning. Once you have the foundation in place, you can gradually implement additional governance pillars as described above to optimize your sandbox environment.</p> 
<hr /> 
<h3>About the authors</h3> 
<footer> 
 <div class="blog-author-box"> 
  <div class="blog-author-image">
   <img alt="" class="wp-image-63082 alignleft" height="160" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/10/10/Kesara-Weerathunga.jpg" width="120" />
  </div> 
  <h3 class="lb-h4">Kesara Weerathunga</h3> 
  <p style="text-align: left;">Kesara Weerathunga is a Senior Software Engineer in the Mendix CCoE team, with a special interest in cloud governance and FinOps. With over a decade of hands-on engineering and architectural experience, he enjoys working with teams to build efficient and scalable cloud solutions. Outside of work, Kesara enjoys exploring new cultures through travel and spending time with his kids and family.</p> 
 </div> 
</footer> 
<footer> 
 <div class="blog-author-box"> 
  <div class="blog-author-image">
   <img alt="" class="wp-image-63083 alignleft" height="160" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/10/10/Jiri-Kostner-.jpg" width="120" />
  </div> 
  <h3 class="lb-h4">Jiri Kostner</h3> 
  <p style="text-align: left;">Jiri Kostner is a Tech Lead in Mendix cloud governance program with over 10+ years of experience in cybersecurity, particularly in cloud security architecture and project management. He is passionate about helping teams solve complex challenges at scale—both within and beyond cybersecurity. Outside of work, he enjoys traveling, reading adventure novels, and spending time with friends and family over a cup of coffee.</p> 
 </div> 
</footer> 
<footer> 
 <div class="blog-author-box"> 
  <div class="blog-author-image">
   <img alt="" class="wp-image-63084 alignleft" height="160" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2025/10/10/Ayush-Bajoria-.jpg" width="120" />
  </div> 
  <h3 class="lb-h4">Ayush Bajoria</h3> 
  <p style="text-align: left;">Ayush Bajoria is a senior DevSecOps Engineer in Mendix specializing in cloud security, with a strong focus on integrating security practices into the software development lifecycle. His work involves identifying and mitigating emerging cyber threats, with a particular emphasis on cloud-native applications and infrastructure. Ayush enjoys traveling and spending quality time with his family.</p> 
 </div> 
</footer>
