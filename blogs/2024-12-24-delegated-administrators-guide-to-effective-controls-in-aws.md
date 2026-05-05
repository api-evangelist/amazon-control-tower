---
title: "Delegated Administrators Guide to Effective Controls in AWS Organizations"
url: "https://aws.amazon.com/blogs/mt/delegated-administrators-guide-to-effective-controls-in-aws-organizations/"
date: "Tue, 24 Dec 2024 17:22:49 +0000"
author: "Craig Edwards"
feed_url: "https://aws.amazon.com/blogs/mt/category/management-tools/aws-control-tower/feed/"
---
<h2>Introduction</h2> 
<p><a href="https://aws.amazon.com/organizations/">AWS Organizations</a> provides the capability to centrally manage and govern your AWS environment. As an organization, you can delegate administration of specific AWS services integrated with AWS Organizations to authorized individuals or teams. Implementing effective controls for these delegated administrators is essential to ensuring the security, compliance, and operational efficiency of your AWS environment.</p> 
<p>This blog post outlines best practices for using <a href="https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html">IAM policy conditions</a>, <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html">Permission boundaries</a>, and <a href="https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html">Service Control Policies (SCPs)</a> to enhance control and governance in AWS Organizations. By following this guidance, delegated administrators can:</p> 
<ul> 
 <li>Apply fine-grained access controls through the use of IAM policy conditions in identity-based policies</li> 
 <li>Limit the maximum permissions of IAM users and roles using permission boundaries</li> 
 <li>Control the maximum available permissions for the IAM users and IAM roles in your organization through the application of Service Control Policies (SCPs)</li> 
 <li>Implement a layered approach to controls that provides redundancy and mitigates potential gaps or errors</li> 
 <li>Streamline the management of controls across multiple accounts and Organizational Units (OUs)</li> 
 <li>Simplify the process of adhering to your organization’s security policies and compliance requirements</li> 
</ul> 
<h3>Getting Started with Controls</h3> 
<p>Let’s dive into how these control mechanisms can be effectively implemented and managed in your AWS environment. We’ll explore best practices, real-world examples, and key considerations for each type of control.</p> 
<p>Implementing layered controls can provide additional safeguards and redundancy. However, it’s important to balance the added security benefits with the complexity of managing multiple overlapping policies. We’ll discuss how to find the right approach for your specific needs.</p> 
<h3>Understanding Preventative Controls</h3> 
<p>Preventative controls in AWS are predefined rules or policies that ensure resources adhere to organizational policies and best practices.</p> 
<ul> 
 <li><strong>Conditional Statements:</strong> Conditional statements in policies allow you to specify conditions under which a policy is in effect, providing finer-grained control over permissions.</li> 
 <li><strong>Permission Boundaries:</strong> Permission boundaries are managed policies that control the maximum permissions a principal (IAM user or role) can have. They limit the permissions granted by the principal’s identity-based policies.</li> 
 <li><strong>Service Control Policies (SCPs):</strong> SCP’s are policies that specify an account’s maximum permissions. They can be expressed in JSON or YAML and are attached to organizational units (OUs) or accounts within your AWS Organizations to help enforce permission controls across your organization.</li> 
</ul> 
<p>Each mechanism – conditional statements, permission boundaries, and SCPs – has its strengths and can be used independently or in combination. A layered approach provides additional safeguards but may increase complexity. The optimal approach depends on your specific requirements, environment complexity, and internal processes. Carefully consider the tradeoffs between added security benefits and the overhead of managing multiple overlapping policies. Work closely with stakeholders to choose the right balance of control mechanisms that effectively enforce your security and compliance standards.</p> 
<div class="wp-caption aligncenter" id="attachment_58780" style="width: 310px;">
 <img alt="Diagram showing the overlap of SCP's, Identity-based policies, and permissions boundaries to create effective permissions" class="wp-image-58780 size-medium" height="246" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2024/12/23/Screenshot-2024-12-23-at-5.41.22 PM-300x246.png" width="300" />
 <p class="wp-caption-text" id="caption-attachment-58780">Figure 1: Layered approach to effective permissions.</p>
</div> 
<h2>Conditional Statements</h2> 
<p>Conditions set specific parameters to allow or deny permissions in a policy. They can leverage resource tags, service-specific conditions, or global condition keys.&nbsp; For more information on the available operators and applications of conditions elements in IAM policies, see IAM JSON policy elements: Condition in the <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html">AWS Identity and Access Management User Guide</a>.</p> 
<p><a href="https://aws.amazon.com/config/">AWS Config</a> allows you to assess, audit, and evaluate the configurations of your AWS resources and is essential for multi-account infrastructures, especially when used with <a href="https://aws.amazon.com/controltower/">AWS Control Tower</a> for detective controls. AWS Control Tower makes it easy for you to set up and govern a secure, multi-account AWS environment.&nbsp; Let’s explore how to apply conditional statements, permission boundaries, and Service Control Policies (SCPs) to AWS Config..</p> 
<p>Using Conditional Statements with Resource Tags: Resource tags are key-value pairs attached to AWS resources, including Identity and Access Management (IAM) users and roles. They enable more granular access control policies based on tags associated with the principal or resource.</p> 
<p>The example below restricts AWS Config access by Department. The following policy grants “config:*” permissions only to users tagged with “department”: “audit”.</p> 
<div class="hide-language"> 
 <pre><code class="lang-json">{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "config:*",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:PrincipalTag/department": "audit"
        }
      }
    }
  ]
}
</code></pre> 
</div> 
<p>This policy ensures only audit team members can perform AWS Config operations, enhancing access control precision.</p> 
<p>There are several service-specific conditions, as well as, global condition keys that can be used to apply conditional statements to a policy. For more information, visit <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-principal-properties">AWS global condition context keys</a> in the <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html">AWS Identity and Access Management User Guide</a>.</p> 
<h3>Best Practices for Using Conditional Statements</h3> 
<ol> 
 <li><strong>Specify Conditions for Access:</strong> Use conditions in policies to specify when and how permissions are granted. (e.g., IP addresses, time of day, multi-factor authentication)</li> 
 <li><strong>Use Tags for Resource-Based Conditions:</strong> Implement tag-based conditions to control access to resources. This practice allows you to manage permissions based on resource tags.</li> 
 <li><strong>Combine Multiple Conditions:</strong> Use multiple conditions to create more granular policies. Combining conditions can help enforce strict access controls.</li> 
 <li><strong>Audit and Update Conditions:</strong> Regularly audit policies with conditional statements to ensure they remain effective and relevant. Update conditions as your organizational requirements change.</li> 
</ol> 
<h2>Permission Boundaries</h2> 
<p>Consider a large financial services company delegating AWS Config administration to an audit team. The company needs to ensure these auditors can perform necessary operations while limiting their maximum permissions to prevent unintended or malicious actions.</p> 
<p>Permission boundaries define the maximum permissions a principal (IAM user or role) can have, regardless of their identity-based policies.</p> 
<p><strong>Step 1: Create a Permission Boundary Policy</strong></p> 
<p>The security team creates a policy that allows all AWS Config operations but explicitly denies the ability to delete Config rules, recorders, or delivery channels. This ensures auditors can manage day-to-day operations without disabling critical Config infrastructure.</p> 
<p>Example Permission Boundary Policy:</p> 
<ol> 
 <li>Log in to the AWS Management Console.</li> 
 <li>Navigate to the IAM service.</li> 
 <li>Create a new policy with the following JSON:</li> 
</ol> 
<div class="hide-language"> 
 <pre><code class="lang-json">{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "config:DeleteConfigRule",
        "config:DeleteOrganizationConfigRule",
        "config:DeleteAggregationAuthorization",
        "config:DeleteConfigurationRecorder",
        "config:DeleteDeliveryChannel"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "config:*"
      ],
      "Resource": "*"
    }
  ]
}

</code></pre> 
 <p><strong>Step 2: Create a Permission Set in AWS IAM Identity Center</strong></p> 
 <p>If the financial services company is using <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html">AWS IAM Identity Center</a>, the next step is to create a permission set and apply the permission boundary:</p> 
 <ol> 
  <li>Navigate to the AWS IAM Identity Center service.</li> 
  <li>Create a new permission set for the AWS Config Auditor role.</li> 
  <li>Attach necessary policies (e.g., AWS Config Administrator policy) to the permission set.</li> 
  <li>Edit the permission set and specify the previously created permission boundary policy.</li> 
 </ol> 
 <p>Benefits of This Approach: Limits auditors’ access to defined boundaries, even if identity-based policies grant more permissions; Aligns with the principle of least privilege; Provides confidence in delegating AWS Config administration; Ensures controlled access limited to necessary actions.</p> 
 <p>By implementing this layered approach, the financial services company can effectively delegate AWS Config administration while maintaining strict control over critical operations. This strategy balances operational needs with security requirements, providing a robust framework for managing permissions in a complex environment.</p> 
 <h3>Best Practices for Using Permission Boundaries</h3> 
 <ol> 
  <li><strong>Establish Clear Boundaries:</strong> Define permission boundary policies that explicitly limit the maximum permissions for IAM roles and users. Align these boundaries with your organization’s security policies and operational needs.</li> 
  <li><strong>Enforce the Boundaries:</strong> When creating or updating IAM principals (roles and users), always attach the appropriate permission boundary policies. This ensures the principals cannot exceed the defined limits, even if their identity-based policies would grant them more permissions.</li> 
  <li><strong>Leverage Identity-Based Policies:</strong> Use permission boundaries in conjunction with identity-based policies to grant the necessary permissions while still enforcing the maximum allowable access.</li> 
  <li><strong>Maintain Vigilance:</strong> Regularly review and update permission boundary policies to reflect changes in your organization’s security requirements, compliance needs, and operational processes. This helps keep your guardrails effective and up-to-date.</li> 
 </ol> 
 <h2>Service Control Policies (SCPs)</h2> 
 <p>Service control policies (SCPs) offer the ability to deny specific actions across an entire organization or Organizational Unit (OU) and apply guardrails to delegated administrator accounts and prevent overly broad permissions. Particularly useful for AWS Config, which allows multiple delegated administrator accounts.</p> 
 <p>AWS Config integrates with services like Control Tower, enabling detective controls through compliance checks. However, this integration presents risks around Administrators could potentially deleting or modifying rules applied through Control Tower. This could cause drift or compromise security standards and compliance requirements.</p> 
 <h3>Prevent users from disabling AWS Config or changing its rules</h3> 
 <p>This SCP prevents users or roles in any affected account from running AWS Config operations that could disable AWS Config or alter its rules or triggers.</p> 
 <pre><code class="lang-json">{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "config:DeleteConfigRule",
        "config:DeleteConfigurationRecorder",
        "config:DeleteDeliveryChannel",
        "config:StopConfigurationRecorder"
      ],
      "Resource": "*"
    }
  ]
}
</code></pre> 
 <h3>Best Practices for Using SCPs</h3> 
 <ol> 
  <li><strong>Start Restrictive, Then Expand Thoughtfully:</strong> Begin with a baseline SCP that enforces a strict least-privilege model across your organization. This minimizes the risk of granting excessive permissions upfront. As your needs evolve, add permissions incrementally, reviewing the impact on your security and compliance posture.</li> 
  <li><strong>Protect Critical Operations:</strong> Use explicit deny statements in SCPs to prevent actions that could compromise the integrity of your AWS environment. For example, you could deny the ability to delete AWS Config rules or stop the Config configuration recorder, ensuring your detective controls remain intact.</li> 
  <li><strong>Leverage Organizational Hierarchy:</strong> Group accounts with similar permission requirements into Organizational Units (OUs). Apply SCPs at the OU level to streamline management and ensure consistent policy enforcement across related accounts.</li> 
  <li><strong>Automate Monitoring and Auditing:</strong> Integrate your SCP management with <a href="https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-add-delegated-administrator.html">AWS CloudTrail</a> and AWS Config to continuously monitor for unauthorized changes or policy violations. This allows you to proactively identify and address any drift from your intended security and compliance standards.</li> 
  <li><strong>Collaborate Across Teams:</strong> Involve stakeholders from security, compliance, and operations teams when defining and reviewing your SCP strategy. This cross-functional approach helps ensure the policies align with your organization’s evolving needs and industry best practices.</li> 
 </ol> 
 <h2>Conclusion</h2> 
 <p>Implementing guardrails using SCPs, permission boundaries, and conditional statements is crucial for maintaining a secure and compliant AWS environment. By following these best practices, you can ensure that your organization’s environment operate within defined security and compliance boundaries while enabling delegated administrators to perform their tasks efficiently.</p> 
 <h3>References</h3> 
 <ul> 
  <li><a href="https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html">AWS Organizations Documentation</a></li> 
  <li><a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html">AWS Identity and Access Management (IAM) Documentation</a></li> 
  <li><a href="https://docs.aws.amazon.com/config/latest/developerguide/WhatIsConfig.html">AWS Config Documentation</a></li> 
 </ul> 
 <h3>Next Steps</h3> 
 <ul> 
  <li>Implement the best practices for conditional statements, permission boundaries, and SCPs discussed in this post to enhance your AWS Organizations governance. <a href="https://docs.aws.amazon.com/organizations/latest/userguide/orgs_best-practices.html">Start with the AWS Organizations Best Practices guide</a>.</li> 
  <li>Set up continuous monitoring of your AWS environment to ensure compliance with your organization’s policies. Consider using <a href="https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config.html">AWS Config rules</a> for this purpose.</li> 
  <li>Regularly review and update your access control strategies to align with evolving business needs and security best practices. Use the <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html">IAM Access Analyzer</a> to help identify unintended access.</li> 
 </ul> 
</div> 
<h3>Additional Resources</h3> 
<ul> 
 <li><a href="https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html">Service Control Policies (SCPs)</a></li> 
 <li><a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html">IAM Permission Boundaries</a></li> 
 <li><a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html">IAM Policy Elements: Condition</a></li> 
 <li><a href="https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html">AWS CloudTrail Documentation</a></li> 
 <li><a href="https://aws.amazon.com/architecture/security-identity-compliance/?cards-all.sort-by=item.additionalFields.sortDate&amp;cards-all.sort-order=desc&amp;awsf.content-type=*all&amp;awsf.methodology=*all">AWS Security Best Practices</a></li> 
</ul> 
<footer> 
 <div class="blog-author-box"> 
  <div class="blog-author-image">
   <img alt="Abraham Musa author photo" class="wp-image-11636 alignleft" height="160" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2023/11/26/abeMusa.png" width="120" />
  </div> 
  <h3 class="lb-h4">Abraham Musa</h3> 
  <p style="text-align: left;">Abraham is a Cloud Operations Specialist Solutions Architect with the Cloud Foundations team at AWS based out of New York. He specializes in AWS Control Tower, AWS Organizations, AWS Service Catalog, and AWS Config. Abraham is a United States Army Veteran and enjoys traveling.</p> 
 </div> 
</footer>
