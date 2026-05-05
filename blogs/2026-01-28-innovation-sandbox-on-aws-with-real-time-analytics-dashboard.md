---
title: "Innovation sandbox on AWS with real-time analytics dashboard"
url: "https://aws.amazon.com/blogs/mt/innovation-sandbox-on-aws-with-real-time-analytics-dashboard/"
date: "Wed, 28 Jan 2026 20:58:09 +0000"
author: "Erkin Ekici"
feed_url: "https://aws.amazon.com/blogs/mt/category/management-tools/aws-control-tower/feed/"
---
<p>How do you deploy hundreds of AWS accounts for a large-scale hackathon? Provide real-time visibility to leadership? Enable participant self-service while monitoring spending across accounts?</p> 
<p>Enterprise innovation events often lack real-time visibility into participant engagement, resource utilization, and outcomes. Leaders can’t see engagement metrics; builders can’t access accounts and information on-demand. Without observability and governance, teams are limited in what they can accomplish.</p> 
<p>Our solution combines <a href="https://aws.amazon.com/solutions/implementations/innovation-sandbox-on-aws/">Innovation Sandbox on AWS</a> for secure account governance with <a href="https://github.com/aws-samples/aws-control-tower-automate-account-creation">AWS Control Tower Automate Account Creation</a> for rapid provisioning and a custom analytics dashboard powered by <a href="https://aws.amazon.com/q/business/">Amazon Q Business</a> generative AI assistant. Self-service accounts with enterprise controls enabled participants to experiment with sensitive data processing—accelerating AI adoption while maintaining compliance. This approach transforms innovation events from black-box experiences into data-driven initiatives with measurable outcomes.</p> 
<p>Our solution solved the core challenge: enabling large-scale AI innovation with enterprise data, governance, and real-time visibility. For builders: 246 AWS accounts provisioned in under 4 hours, plus self-service resources (knowledge base, generative AI assistant, expert support form) serving 213 participants. For leadership: real-time visibility across 23 sessions, with peak attendance reaching 153 in keynotes and 41 in technical workshops.</p> 
<h2>The challenge</h2> 
<p>A major European telecommunications provider with millions of customers had a large-scale generative AI hackathon with a challenge: enabling 500+ participants across 100+ teams to rapidly develop AI innovations while maintaining enterprise-grade security and governance. With just weeks until the event launch, the team faced significant technical and operational hurdles that traditional account provisioning approaches simply couldn’t solve.</p> 
<p>The scale and complexity of the initiative demanded an innovative solution:</p> 
<ul> 
 <li><strong>Massive concurrent account creation</strong> – Creating over 200 secure AWS accounts in days rather than the weeks or months typically required through standard processes</li> 
 <li><strong>Comprehensive security controls</strong> – Maintaining proper isolation for each account while implementing organization-wide security policies, spending limits, and access controls</li> 
 <li><strong>Diverse technical expertise</strong> – Supporting participants with varying cloud experience levels, from AI researchers to business analysts joining from different continents, requiring intuitive self-service capabilities</li> 
 <li><strong>Real-time operational visibility</strong> – Providing executives with AI-assisted dashboard tracking participant engagement, resource utilization, and technical adoption metrics along with providing participants information on agenda, contact form and knowledge base</li> 
 <li><strong>Data governance requirements: </strong>AI development with internal data required accounts under customer’s own governance, not externally managed environments.</li> 
</ul> 
<p>Critically, the solution had to be production-ready within hours—a timeline that would be challenging even for small-scale deployments. This tight schedule left no margin for error in design, implementation, or testing phases.</p> 
<p>The technical team recognized that addressing these challenges required more than just accelerating existing processes. It demanded a different approach combining AWS account governance, automated provisioning, and real-time analytics.</p> 
<h2>Solution architecture</h2> 
<p>Our solution leveraged <a href="https://aws.amazon.com/solutions/implementations/innovation-sandbox-on-aws/">Innovation Sandbox on AWS</a> as the foundation, enhanced with custom automation and real-time analytics capabilities. Innovation Sandbox on AWS provided the architectural patterns and security controls, while additional components handled rapid account creation and executive visibility. The architecture consisted of three core components: automated account provisioning, self-service access portal, and executive analytics dashboard.</p> 
<p>We implemented the solution with <a href="https://kiro.dev/docs/cli/">Kiro CLI</a> using prompt engineering and human oversight. You can find the example prompts throughout this post. Rather than sharing traditional code snippets, we’re providing the actual prompts used with <a href="https://kiro.dev/cli/">Kiro CLI</a>. These prompts generated the dashboard components, API integrations, and infrastructure code needed for the solution.</p> 
<div class="wp-caption alignnone" style="width: 1451px;">
 <img alt="Architecture diagram showing self-service dashboard with Amazon Q Business integration: executives and participants access CloudFront-hosted dashboard with S3 static assets, API Gateway and Lambda generate Q Business URLs, while Innovation Sandbox on AWS in the management account provisions accounts via AWS Control Tower and Organizations into Entry OU. Kiro CLI generates dashboard code deployed via AWS CDK with data sync scripts." height="891" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2026/01/14/self-service-dashboard-solutions-architecture-1.png" width="1441" />
 <p class="wp-caption-text">Solution architecture</p>
</div> 
<h3>1. Innovation Sandbox on AWS Foundation</h3> 
<p>We deployed <a href="https://docs.aws.amazon.com/solutions/latest/innovation-sandbox-on-aws/">Innovation Sandbox on AWS</a> to provide essential infrastructure for managing temporary AWS accounts at scale. The solution deploys a specific Organizational Unit (OU) called <code>Entry</code> where accounts can be onboarded into the solution. We configured organizational units with pre-defined security policies, spending controls, and automated cleanup mechanisms. The sandbox environment included service control policies limiting access to sensitive services with optional time and budget controls, while enabling generative AI experimentation with <a href="https://aws.amazon.com/bedrock/">Amazon Bedrock</a>, <a href="https://aws.amazon.com/sagemaker/">Amazon SageMaker</a>, and other AI/ML services.</p> 
<h3>2. Automated Account Creation</h3> 
<p>We used <a href="https://github.com/aws-samples/aws-control-tower-automate-account-creation">AWS Control Tower Automate Account Creation</a> in parallel to quickly deploy hundreds of accounts. This enabled batch account provisioning with enterprise controls. This CloudFormation-based solution uses <a href="https://aws.amazon.com/servicecatalog/">AWS Service Catalog</a> APIs to create multiple accounts simultaneously, reducing provisioning time from hours to seconds per account.</p> 
<h3>3. Custom Analytics Dashboard</h3> 
<p>We built a custom dashboard to provide real-time insights, agenda, knowledge base and contact form with Amazon Q Business integration. The custom analytics dashboard combined data collection with Amazon Q Business integration for intelligent insights. The architecture used:</p> 
<ul> 
 <li><a href="https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/getting-started-cloudfront-overview.html">Static web hosting on S3 with CloudFront</a> distribution</li> 
 <li><a href="https://aws.amazon.com/lambda/">Lambda</a> functions for <a href="https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-exp-actions-anonymous.html">Amazon Q Business URL generation</a></li> 
 <li><a href="https://aws.amazon.com/api-gateway/">API Gateway</a> for <a href="https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-cors.html">CORS-enabled</a> endpoints</li> 
 <li>JavaScript modules for real-time metric calculations</li> 
 <li>Automated data aggregation for <a href="https://docs.aws.amazon.com/quicksuite/latest/userguide/knowledge-base-integrations.html">Amazon Q Business knowledge base</a></li> 
</ul> 
<h3>AWS Well-Architected Framework Alignment</h3> 
<p>The solution followed <a href="https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html">AWS Well-Architected Framework</a> principles:</p> 
<ul> 
 <li><strong>Operational Excellence:</strong> Automated provisioning and real-time monitoring reduced manual effort.</li> 
 <li><strong>Security</strong>: Pre-configured policies and isolated sandbox accounts protected resources.</li> 
 <li><strong>Cost Optimization</strong>: Automated cleanup and spending controls minimized waste.</li> 
 <li><strong>Performance Efficiency</strong>: Parallelized account creation accelerated deployment.</li> 
 <li><strong>Sustainability:</strong> Automated decommissioning reduced idle resource consumption.</li> 
</ul> 
<h2>Walkthrough</h2> 
<p>Working closely with the customer’s cloud and AI teams, we rapidly iterated on the account provisioning strategy to meet their specific security and governance requirements, all within 3 days.</p> 
<h3>Setting Up the Sandbox Environment</h3> 
<p>We followed the <a href="https://docs.aws.amazon.com/solutions/latest/innovation-sandbox-on-aws/">Innovation Sandbox on AWS Implementation Guide</a> including prerequisites for <a href="https://aws.amazon.com/ram/">Resource Access Manager (RAM)</a> for cross-account sharing and <a href="https://aws.amazon.com/ses/">Amazon Simple Email Service</a> (Amazon SES) for participant notifications. We<a href="https://docs.aws.amazon.com/solutions/latest/innovation-sandbox-on-aws/update-auth-config.html"> configured AppConfig</a> with customer-specific configuration values and created a single lease for participants to use. A single person managed both <a href="https://docs.aws.amazon.com/solutions/latest/innovation-sandbox-on-aws/administrator-guide.html">administrator</a> (for account pool and settings management) and <a href="https://docs.aws.amazon.com/solutions/latest/innovation-sandbox-on-aws/manager-guide.html">manager</a> (for lease templates and approvals) roles, streamlining operations.</p> 
<h3>Provisioning the Accounts</h3> 
<p>We deployed the <a href="http://AWS Control Tower Automate Account Creation">AWS Control Tower Automate Account Creation</a> stack in the management account, configuring it to provision accounts into a dedicated <code>Entry</code> organizational unit deployed by Innovation Sandbox on AWS.</p> 
<p>To meet the 3-day timeline, we parallelized the provisioning by deploying multiple CloudFormation stacks simultaneously—each stack handling a subset of accounts. This approach doubled our provisioning throughput, creating 246 AWS accounts for 213 confirmed participants in under 4 hours instead of the longer time needed for sequential processing.</p> 
<p>We generated different CSV files containing information for 400 dummy accounts, each with a unique account name, dedicated email address, SSO email, username for access, and the specific organizational unit for the account. We placed CSV files in an S3 bucket for the automation to access. We then deployed <a href="https://github.com/aws-samples/aws-control-tower-automate-account-creation">BatchAccountCreation.yaml</a> CloudFormation stack for each CSV file, which processed the corresponding accounts file, creating each account accordingly. This system allowed us to focus on other tasks while the accounts were being created.</p> 
<h3>Building the Monitoring Dashboard</h3> 
<p>The dashboard provided a web-based interface, enabling participants to:</p> 
<ul> 
 <li>View quick-start guides and tutorials</li> 
 <li>Visualize key metrics and participants information</li> 
 <li>Show agenda and demos</li> 
 <li>Send use-case submissions to request additional support</li> 
 <li>Get AI-powered assistance through embedded Amazon Q Business</li> 
</ul> 
<p>To provide the dashboard quickly, we used <a href="https://kiro.dev/docs/cli/">Kiro CLI</a> for rapid coding, this guide will provide the prompts used to build similar solutions.</p> 
<p>Example prompt for generating AWS <a href="https://docs.aws.amazon.com/whitepapers/latest/introduction-devops-aws/infrastructure-as-code.html">Infrastructure as Code</a> using <a href="https://aws.amazon.com/cdk/">AWS CDK</a>:</p> 
<pre><code class="language-txt">[Role]
You are an AWS Solutions Architect expert in AWS CDK (Cloud Development Kit) and serverless dashboard architectures.
[Task]
Generate a complete AWS CDK application in TypeScript that provisions secure dashboard hosting with Q Business API integration and CloudFront distribution.
[Code Output Format]
- TypeScript CDK stack with proper construct dependencies
- Minimal IAM permissions following least privilege principle
- Stack outputs for deployment automation integration
[Requirements]
- S3: Private bucket with versioning, blocked public access using BucketDeployment
- CloudFront: Distribution with Origin Access Identity, API Gateway integration
 Lambda Function: Python function construct for Q Business URL generation with boto3
- API Gateway: LambdaRestApi with CORS configuration for /qbusiness-url endpoint
- Security: Role constructs with minimal qbusiness:CreateAnonymousWebExperienceUrl permissions
[Instructions]
1. MUST use OriginAccessIdentity construct for S3-CloudFront security
2. MUST implement BehaviorOptions: S3 for static files, API Gateway for /api/* paths
3. MUST configure CorsOptions for browser API requests
4. MUST export CfnOutput for bucket name, CloudFront URL, distribution ID
5. DO NOT use overly permissive PolicyStatement - scope to specific Q Business application ARN
[Success Criteria]
- CDK stack deploys successfully with cdk deploy in any AWS region
- S3 bucket remains private with CloudFront-only access via OAI
- API Gateway returns valid Q Business URLs with proper CORS headers
- All stack outputs available for deployment automation scripts
</code></pre> 
<h4>Data Collection</h4> 
<p>We followed a GitOps approach to update data—a single JSON file served as the source of truth, with datapoints automatically cross-tested to detect failures early. The dashboard used this aggregated data to update Amazon Q Business document, and provide real-time calculations/presentation from the static website. The data was updated continuously during the hackathon.</p> 
<p>Our dashboard aggregated data from multiple sources to provide comprehensive event visibility. Session attendance tracked through virtual meeting platforms. Real-time engagement was calculated from session participation. Working with the customer’s Lead Cloud Architect, we collected AWS account cost metrics from Organization Unit (OU) where account exists, and Kiro Pro subscriptions count from Kiro dashboard. Innovation Sandbox on AWS provided isolated Organizational Unit (OU)s which made the calculation easy.</p> 
<p>Example prompt for real-time data processing engine:</p> 
<pre><code class="language-txt">[Role]
You are a data processing specialist creating real-time analytics engines for executive dashboards with trend analysis.
[Task]
Build a JavaScript module that processes hackathon session data, calculates KPIs, and generates dynamic UI components with
statistical analysis.
[Code Output Format]
- Modular JavaScript functions for data calculations
- JSON data structure as centralized source of truth
- Dynamic DOM manipulation for real-time updates
[Requirements]
- Peak Calculations: Technical session peak, non-technical peak from session data
- Trend Analysis: Day-over-day percentage changes with directional indicators
- KPI Generation: Engagement rate (53%), AWS adoption (21%), Kiro Pro conversion (7%)
- Dynamic Rendering: Session cards with participant counts and click handlers
- Data Structure: Comprehensive data.json with sessions, accounts, and statistics
[Instructions]
1. MUST calculate all statistics from data.json - no hardcoded values
2. MUST implement percentage change calculations with null value handling
3. MUST generate session cards dynamically with modal integration
4. MUST validate data consistency across all calculations
5. DO NOT duplicate data - maintain single source of truth in JSON structure
[Success Criteria]
- All KPIs calculate correctly from source data
- Percentage trends display proper directional indicators (↑↓)
- Session cards render dynamically with accurate participant counts
- Data validation prevents calculation errors and handles edge cases
</code></pre> 
<p>These metrics allowed us to set the following KPIs, and through those monitor the health of the hackathon:</p> 
<ul> 
 <li>Peak session attendance (technical vs. business tracks)</li> 
 <li>Day-over-day engagement trends</li> 
 <li>AWS service adoption rates</li> 
 <li>Kiro subscription metrics</li> 
 <li>Account utilization and spending patterns</li> 
</ul> 
<div class="wp-caption aligncenter" style="width: 684px;">
 <img alt="Day 1 hackathon dashboard showing session cards with participant counts: business sessions including Introduction (353), Re:Invent with GenAI (280), Generative AI for Telco (234); technical sessions including Getting Started with AWS (24), Kiro CLI Workshop (23), Ask an Architect (29); plus account metrics showing 3 Kiro Pro subscriptions, 38 active AWS accounts, and $10 total spending" class="" height="409" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2026/01/14/dashboard-agenda-day-1.png" width="674" />
 <p class="wp-caption-text">Real-time dashboard showing Day 1</p>
</div> 
<p>Given the generative AI focus of the hackathon, we specifically tracked Kiro subscriptions as a key productivity indicator. Participants who activated Kiro Pro after technical workshops demonstrated commitment to accelerating their development cycles and building enterprise-grade AI development skills—critical capabilities for scaling generative AI initiatives beyond the event. To support generative AI adoption, we provided Kiro installation guides within the dashboard, while the customer’s AI research team created additional documentation integrated into the dashboard resources.</p> 
<p>A critical success factor was making technical metrics meaningful to business stakeholders. The dashboard included contextual explanations for each metric:</p> 
<ul> 
 <li><strong>AWS Spending Increases</strong>: Translated as “more complex solutions using advanced services” and “indicates solution complexity and innovation depth”</li> 
 <li><strong>Kiro Pro Subscriptions</strong>: Explained as “participants adopting advanced development tools” that “accelerates development cycles and builds enterprise-grade AI skills”.</li> 
 <li><strong>Account Usage Growth</strong>: Clarified as “more participants joining hands-on development” providing “practical cloud development experience”.</li> 
</ul> 
<table> 
 <tbody> 
  <tr> 
   <td width="235"> <p></p>
    <div class="wp-caption alignnone" style="width: 287px;">
     <img alt="Dashboard modal explaining Kiro Pro Subscriptions metric tracking participant adoption of advanced development tools after technical workshops, with trend analysis and benefits for accelerating development cycles" class="" height="247" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2026/01/14/dashbord-kiro-pro.png" width="277" />
     <p class="wp-caption-text">Kiro Pro metric explanation</p>
    </div></td> 
   <td width="235"> <p></p>
    <div class="wp-caption alignnone" style="width: 288px;">
     <img alt="Dashboard modal explaining AWS Accounts metric with trend analysis showing increase means more participants joining development, decrease means account cleanup, and why it matters for cloud development experience" class="" height="247" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2026/01/14/dashboard-aws-accounts.png" width="278" />
     <p class="wp-caption-text">AWS Accounts metric explanation</p>
    </div></td> 
   <td width="235"> <p></p>
    <div class="wp-caption alignnone" style="width: 293px;">
     <img alt="Dashboard modal explaining AWS Spending metric showing total resource consumption across participant accounts, with trend analysis indicating increases mean more complex solutions and decreases mean optimization phases" class="" height="252" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2026/01/14/dashboard-aws-spending.png" width="283" />
     <p class="wp-caption-text">AWS Spending metric explanation</p>
    </div></td> 
  </tr> 
 </tbody> 
</table> 
<h4>Knowledge Base and Self-Service Resources</h4> 
<p>Beyond metrics tracking, the dashboard ecosystem included three critical self-service components that reduced administrative overhead while improving participant experience.</p> 
<p>The wiki served as a knowledge base with quick links to essential tools, setup guides, and frequently asked questions, becoming the first line of information sharing among participants during sessions.</p> 
<p>The interactive agenda helped users navigate the complex 5-day schedule across technical and business tracks, with Amazon Q Business integration providing personalized session recommendations based on participant roles and interests.</p> 
<p>The Submit Use Case form enabled teams to request additional AWS expert support directly, connecting promising projects with specialized technical guidance.</p> 
<p>Example prompt for executive dashboard with real-time KPIs:</p> 
<pre><code class="language-txt">[Role]
You are a senior full-stack developer specializing in AWS-themed executive dashboards and real-time data visualization.
[Task]
Create a comprehensive executive dashboard HTML page that displays hackathon metrics with interactive KPI cards, day-by-day session tracking, and modal functionality.
[Code Output Format]
- Single HTML file with embedded CSS and JavaScript
- Modular functions for data processing and UI updates
- Responsive design optimized for executive viewing
[Requirements]
- AWS Design System: Use #232F3E dark blue, #FF9900 orange, Amazon Ember font
- KPI Cards: Peak Technical Session (82), Peak Non-Technical (353), Engagement Rate (73%), AWS Adoption (41%), Kiro Pro Adoption (70%)
- Daily Tracking: 5-day sections with session cards showing participant counts
- Interactive Elements: Clickable session cards opening detailed modals
- Data Source: Load all metrics from data.json as single source of truth
[Instructions]
- MUST implement CSS Grid layout with hover effects and smooth transitions
- MUST calculate day-over-day percentage changes with trend indicators (↑↓)
- MUST create modal system with session details (time, track, format, description)
- MUST handle null participant values gracefully
- DO NOT hardcode any metrics - calculate from centralized JSON data
[Success Criteria]
- All KPIs display with correct trend calculations
- Session modals open smoothly with complete information
- Mobile responsive design works on 320px+ screens
- Real-time updates when data.json changes
</code></pre> 
<table> 
 <tbody> 
  <tr> 
   <td width="229"> <p></p>
    <div class="wp-caption alignnone" style="width: 227px;">
     <img alt="GenAI Hackathon dashboard showing Submit AI Use Case form with email address field and use case description textarea, allowing participants to share ideas with the AWS team for review" height="240" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2026/01/14/dashboard-use-case.png" width="217" />
     <p class="wp-caption-text">Use case submission</p>
    </div></td> 
   <td width="232"> <p></p>
    <div class="wp-caption alignnone" style="width: 246px;">
     <img alt="Hackathon agenda for Monday September 1 showing two keynote sessions: Introduction, goals and mechanics (09:00-09:30, 353 participants) and Re:Invent with GenAI (09:30-10:00, 280 participants), with Ask Q about Hackathon button in bottom right" class="" height="245" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2026/01/14/dashbord-agenda.png" width="236" />
     <p class="wp-caption-text">Interactive Agenda</p>
    </div></td> 
   <td width="244"> <p></p>
    <div class="wp-caption aligncenter" style="width: 312px;">
     <img alt="GenAI Hackathon dashboard showing Submit AI Use Case form with email address field and use case description textarea, allowing participants to share ideas with the AWS team for review" class="" height="245" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2026/01/14/dashboard-wiki.png" width="302" />
     <p class="wp-caption-text">Wiki Page</p>
    </div></td> 
  </tr> 
 </tbody> 
</table> 
<h4>Amazon Q Business Integration</h4> 
<p>The dashboard included embedded Amazon Q Business for intelligent event assistance as chatbot. It worked by client showing “Talk to Dashboard” icon on bottom right of the screen.</p> 
<p>Once clicked, the client sent a request to the API (Lambda) that creates a temporary Amazon Q Business URL and once the client received the URL, it dynamically created an iframe that would display the Amazon Q Business interface.</p> 
<p>Example prompt for business AI chat integration:</p> 
<pre><code class="language-txt">[Role]
You are a frontend integration specialist expert in Amazon Q Business and responsive iframe implementations.
[Task]
Create a JavaScript module that integrates Amazon Q Business chat with dynamic URL fetching, responsive UI, and session
management.
[Code Output Format]
- Standalone JavaScript module with CSS styling
- API integration for Q Business URL generation
- Mobile-responsive iframe with error handling
[Requirements]
- Toggle Button: Bottom-right "Ask Q about Hackathon ?" with AWS orange styling
- Dynamic URLs: Fetch fresh Q Business URLs from /api/qbusiness-url to avoid expiration
- Responsive Design: Full-width iframe on mobile (&lt;480px), fixed positioning on desktop
- State Management: Toggle between "Ask Q" and "Close Chat" button states
- Error Handling: Loading indicators, API failure messages, session recovery
[Instructions]
1. MUST fetch new Q Business URL on each chat session opening
2. MUST implement responsive iframe: 450px max-width desktop, full-width mobile
3. MUST show loading indicator during URL generation API calls
4. MUST handle API failures with user-friendly error messages
5. DO NOT cache Q Business URLs - always request fresh URLs for security
[Success Criteria]
- Chat toggles smoothly between open/closed states
- Mobile devices display chat interface properly without overflow
- API failures show helpful messages instead of breaking functionality
- Q Business iframe loads successfully with dashboard context</code></pre> 
<div class="wp-caption aligncenter" style="width: 482px;">
 <img alt="Amazon Q Business chat interface overlaying the hackathon agenda, showing a participant asking which Tuesday session to attend as a data scientist, with Q recommending the Getting Started with AWS Environments session based on their role" height="379" src="https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2026/01/14/dashboard-q.png" width="472" />
 <p class="wp-caption-text">Amazon Q Business integration</p>
</div> 
<h2>Cleanup and Cost Management</h2> 
<h3>Account Decommissioning</h3> 
<p>Post hackathon, the automated cleanup mechanisms in Innovation Sandbox on AWS handled account <a href="https://docs.aws.amazon.com/solutions/latest/innovation-sandbox-on-aws/account-lifecycle-in-isb.html">lifecycle management</a>:</p> 
<ul> 
 <li><a href="https://docs.aws.amazon.com/solutions/latest/innovation-sandbox-on-aws/manager-guide.html"><strong>Frozen Accounts</strong></a><strong>:</strong> Innovation Sandbox automatically revoked user access to AWS accounts after 14 days, while administrators retained access for evaluation</li> 
 <li><a href="https://docs.aws.amazon.com/solutions/latest/innovation-sandbox-on-aws/administrator-guide.html"><strong>Automated Cleanup</strong></a><strong>:</strong> Innovation Sandbox automatically deleted account resources after 21 days, unless explicitly preserved.</li> 
 <li><a href="https://docs.aws.amazon.com/solutions/latest/innovation-sandbox-on-aws/remove-resources.html"><strong>Account Ejection</strong></a>: We moved promising projects to permanent accounts, preserving all resources.</li> 
</ul> 
<h3>Cost Optimization Considerations</h3> 
<ul> 
 <li>We configured Innovation Sandbox budget alerts at USD $50 and USD $100 thresholds.</li> 
 <li>We used pre-configured service control policies from Innovation Sandbox on AWS preventing expensive resource types.</li> 
 <li>We used automated resource tagging for cost allocation.</li> 
 <li>We used the analytics dashboard for spending visibility.</li> 
</ul> 
<p>Total infrastructure costs remained under USD $2,000 for the entire event, with 89% of accounts staying within the USD $100 budget limit.</p> 
<h2>Results and Conclusion</h2> 
<p>The solution delivered measurable results across all dimensions, demonstrating how Innovation Sandbox on AWS can be enhanced with custom analytics to transform enterprise innovation events. Peak engagement reached 153 participants in keynote sessions, with 41 participants in hands-on technical workshops—representing a 53% technical engagement rate.</p> 
<h3>Key Metrics and Outcomes</h3> 
<p><strong>Infrastructure Performance:</strong></p> 
<ul> 
 <li>246 AWS accounts provisioned in under 4 hours</li> 
 <li>Zero security incidents or policy violations</li> 
 <li>Average account setup time reduced from 2+ hours to seconds</li> 
 <li>Total infrastructure costs under USD $2,000 with 89% of accounts staying within USD $100 budget limits</li> 
 <li>Internal data governance maintained – All accounts remained under the customer’s enterprise control, enabling AI development with internal data</li> 
</ul> 
<p><strong>Participant Engagement:</strong></p> 
<ul> 
 <li>21% adopted new <a href="https://aws.amazon.com/ai/">AWS AI services</a></li> 
 <li>7% started using <a href="https://kiro.dev/">Kiro</a></li> 
 <li><a href="https://aws.amazon.com/bedrock/">Amazon Bedrock</a> used in 71% of AWS-based projects</li> 
 <li>34% of business track attendees created functional prototypes</li> 
</ul> 
<p><strong>Innovation Outcomes:</strong></p> 
<p>The hackathon generated 7 AI-powered AWS-based solutions awarded for technical excellence—from AI-powered call center agents serving millions of customers to autonomous network management systems. Customer Experience solutions led performance at 7.7 average score. Enterprise-controlled accounts allowed 86% of solutions to target internal data use cases.</p> 
<h3>Dashboard Impact and Adoption</h3> 
<p>The analytics dashboard served three critical phases: pre-event logistics and account access communication, real-time monitoring during the event, and post-event executive reporting for ROI analysis. As one director noted: “We love the dashboard, I personally refreshed it 20 times daily.” This visibility enabled leadership to make data-driven decisions about resource allocation and future innovation initiatives.</p> 
<h3>Success Factors and Reusability</h3> 
<p>Key success factors included leveraging existing AWS solutions as foundations, building modular analytics for reusability, and integrating Amazon Q Business for intelligent assistance. The self-service approach reduced administrative overhead while empowering participants to extend their learning beyond the event timeline.</p> 
<p>The patterns demonstrated here are reusable across hackathons, training programs, and innovation labs of any scale. Innovation Sandbox on AWS provides the secure foundation, while custom analytics transforms visibility and engagement measurement.</p> 
<h3>Ready to Run Your Innovation Events?</h3> 
<p>Are you ready to upskill your team and push the limits of what’s possible? Enable them to innovate by giving them secure AWS accounts in minutes on-demand, providing self-service access to the information they need. Let leadership see results in real-time and support you. Turn your next innovation event into a launchpad for production-ready solutions.</p> 
<p>Start with <a href="https://aws.amazon.com/solutions/implementations/innovation-sandbox-on-aws/">Innovation Sandbox on AWS</a> and enhance it with custom analytics tailored to your organization’s needs. The combination of automated provisioning, real-time analytics, and AI-powered assistance creates streamlined experiences that enable participants to focus on innovation rather than infrastructure.</p> 
<p><strong>Next Steps:</strong></p> 
<ul> 
 <li><a href="https://docs.aws.amazon.com/solutions/latest/innovation-sandbox-on-aws/">Innovation Sandbox on AWS Implementation Guide</a></li> 
 <li><a href="https://github.com/aws-samples/aws-control-tower-automate-account-creation">AWS Control Tower Automate Account Creation</a></li> 
 <li><a href="https://docs.aws.amazon.com/amazonq/latest/business-use-dg/">Amazon Q Business Integration Documentation</a></li> 
 <li><a href="https://kiro.dev/">Get Started with Kiro</a></li> 
</ul> 
<p>Acknowledgments: Thanks to Shu Jackson, Rakshana Balakrishnan, Todd Gruet, and Kevin Hargita from the Innovation Sandbox team for their support during this project.</p>
