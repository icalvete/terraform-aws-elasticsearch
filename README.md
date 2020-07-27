# terraform-aws-elasticsearch [![Codefresh Build Status](https://g.codefresh.io/api/badges/pipeline/cloudposse/terraform-modules%2Fterraform-aws-elasticsearch?type=cf-1)](https://g.codefresh.io/public/accounts/cloudposse/pipelines/5d22bfe5a7e22ea3b67ea820) [![Latest Release](https://img.shields.io/github/release/cloudposse/terraform-aws-elasticsearch.svg)](https://github.com/cloudposse/terraform-aws-elasticsearch/releases/latest) [![Slack Community](https://slack.cloudposse.com/badge.svg)](https://slack.cloudposse.com)

[![README Header][readme_header_img]][readme_header_link]

[![Cloud Posse][logo]](https://cpco.io/homepage)

<!--




  ** DO NOT EDIT THIS FILE
  **
  ** This file was automatically generated by the `build-harness`.
  ** 1) Make all changes to `README.yaml`
  ** 2) Run `make init` (you only need to do this once)
  ** 3) Run`make readme` to rebuild this file.
  **
  ** (We maintain HUNDREDS of open source projects. This is how we maintain our sanity.)
  **





-->

Terraform module to provision an [`Elasticsearch`](https://aws.amazon.com/elasticsearch-service/) cluster with built-in integrations with [Kibana](https://aws.amazon.com/elasticsearch-service/kibana/) and [Logstash](https://aws.amazon.com/elasticsearch-service/logstash/).


---

This project is part of our comprehensive ["SweetOps"](https://cpco.io/sweetops) approach towards DevOps.
[<img align="right" title="Share via Email" src="https://docs.cloudposse.com/images/ionicons/ios-email-outline-2.0.1-16x16-999999.svg"/>][share_email]
[<img align="right" title="Share on Google+" src="https://docs.cloudposse.com/images/ionicons/social-googleplus-outline-2.0.1-16x16-999999.svg" />][share_googleplus]
[<img align="right" title="Share on Facebook" src="https://docs.cloudposse.com/images/ionicons/social-facebook-outline-2.0.1-16x16-999999.svg" />][share_facebook]
[<img align="right" title="Share on Reddit" src="https://docs.cloudposse.com/images/ionicons/social-reddit-outline-2.0.1-16x16-999999.svg" />][share_reddit]
[<img align="right" title="Share on LinkedIn" src="https://docs.cloudposse.com/images/ionicons/social-linkedin-outline-2.0.1-16x16-999999.svg" />][share_linkedin]
[<img align="right" title="Share on Twitter" src="https://docs.cloudposse.com/images/ionicons/social-twitter-outline-2.0.1-16x16-999999.svg" />][share_twitter]


[![Terraform Open Source Modules](https://docs.cloudposse.com/images/terraform-open-source-modules.svg)][terraform_modules]



It's 100% Open Source and licensed under the [APACHE2](LICENSE).







We literally have [*hundreds of terraform modules*][terraform_modules] that are Open Source and well-maintained. Check them out!






## Introduction

This module will create:
- Elasticsearch cluster with the specified node count in the provided subnets in a VPC
- Elasticsearch domain policy that accepts a list of IAM role ARNs from which to permit management traffic to the cluster
- Security Group to control access to the Elasticsearch domain (inputs to the Security Group are other Security Groups or CIDRs blocks to be allowed to connect to the cluster)
- DNS hostname record for Elasticsearch cluster (if DNS Zone ID is provided)
- DNS hostname record for Kibana (if DNS Zone ID is provided)

__NOTE:__ To enable [zone awareness](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-managedomains.html#es-managedomains-zoneawareness) to deploy Elasticsearch nodes into two different Availability Zones, you need to set `zone_awareness_enabled` to `true` and provide two different subnets in `subnet_ids`.
If you enable zone awareness for your domain, Amazon ES places an endpoint into two subnets.
The subnets must be in different Availability Zones in the same region.
If you don't enable zone awareness, Amazon ES places an endpoint into only one subnet. You also need to set `availability_zone_count` to `1`.

## Usage


**IMPORTANT:** The `master` branch is used in `source` just as an example. In your code, do not pin to `master` because there may be breaking changes between releases.
Instead pin to the release tag (e.g. `?ref=tags/x.y.z`) of one of our [latest releases](https://github.com/cloudposse/terraform-aws-elasticsearch/releases).


Basic [example](examples/basic)

```hcl
module "elasticsearch" {
  source                  = "git::https://github.com/cloudposse/terraform-aws-elasticsearch.git?ref=master"
  namespace               = "eg"
  stage                   = "dev"
  name                    = "es"
  dns_zone_id             = "Z14EN2YD427LRQ"
  security_groups         = ["sg-XXXXXXXXX", "sg-YYYYYYYY"]
  vpc_id                  = "vpc-XXXXXXXXX"
  subnet_ids              = ["subnet-XXXXXXXXX", "subnet-YYYYYYYY"]
  zone_awareness_enabled  = "true"
  elasticsearch_version   = "6.5"
  instance_type           = "t2.small.elasticsearch"
  instance_count          = 4
  iam_role_arns           = ["arn:aws:iam::XXXXXXXXX:role/ops", "arn:aws:iam::XXXXXXXXX:role/dev"]
  iam_actions             = ["es:ESHttpGet", "es:ESHttpPut", "es:ESHttpPost"]
  encrypt_at_rest_enabled = true
  kibana_subdomain_name   = "kibana-es"

  advanced_options = {
    "rest.action.multi.allow_explicit_index" = "true"
  }
}
```






<!-- markdownlint-disable -->
## Makefile Targets
```text
Available targets:

  help                                Help screen
  help/all                            Display help for all targets
  help/short                          This help short screen
  lint                                Lint terraform code

```
<!-- markdownlint-restore -->
## Requirements

| Name | Version |
|------|---------|
| terraform | ~> 0.12.0 |
| aws | ~> 2.0 |
| null | ~> 2.0 |
| template | ~> 2.0 |

## Providers

| Name | Version |
|------|---------|
| aws | ~> 2.0 |
| null | ~> 2.0 |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| advanced\_options | Key-value string pairs to specify advanced configuration options | `map(string)` | `{}` | no |
| allowed\_cidr\_blocks | List of CIDR blocks to be allowed to connect to the cluster | `list(string)` | `[]` | no |
| attributes | Additional attributes (e.g. `1`) | `list(string)` | `[]` | no |
| automated\_snapshot\_start\_hour | Hour at which automated snapshots are taken, in UTC | `number` | `0` | no |
| availability\_zone\_count | Number of Availability Zones for the domain to use. | `number` | `2` | no |
| aws\_ec2\_service\_name | AWS EC2 Service Name | `list(string)` | <pre>[<br>  "ec2.amazonaws.com"<br>]</pre> | no |
| cognito\_authentication\_enabled | Whether to enable Amazon Cognito authentication with Kibana | `bool` | `false` | no |
| cognito\_iam\_role\_arn | ARN of the IAM role that has the AmazonESCognitoAccess policy attached | `string` | `""` | no |
| cognito\_identity\_pool\_id | The ID of the Cognito Identity Pool to use | `string` | `""` | no |
| cognito\_user\_pool\_id | The ID of the Cognito User Pool to use | `string` | `""` | no |
| create\_iam\_service\_linked\_role | Whether to create `AWSServiceRoleForAmazonElasticsearchService` service-linked role. Set it to `false` if you already have an ElasticSearch cluster created in the AWS account and AWSServiceRoleForAmazonElasticsearchService already exists. See https://github.com/terraform-providers/terraform-provider-aws/issues/5218 for more info | `bool` | `true` | no |
| dedicated\_master\_count | Number of dedicated master nodes in the cluster | `number` | `0` | no |
| dedicated\_master\_enabled | Indicates whether dedicated master nodes are enabled for the cluster | `bool` | `false` | no |
| dedicated\_master\_type | Instance type of the dedicated master nodes in the cluster | `string` | `"t2.small.elasticsearch"` | no |
| delimiter | Delimiter to be used between `namespace`, `environment`, `stage`, `name` and `attributes` | `string` | `"-"` | no |
| dns\_zone\_id | Route53 DNS Zone ID to add hostname records for Elasticsearch domain and Kibana | `string` | `""` | no |
| domain\_endpoint\_options\_enforce\_https | Whether or not to require HTTPS | `bool` | `false` | no |
| domain\_endpoint\_options\_tls\_security\_policy | The name of the TLS security policy that needs to be applied to the HTTPS endpoint | `string` | `"Policy-Min-TLS-1-0-2019-07"` | no |
| ebs\_iops | The baseline input/output (I/O) performance of EBS volumes attached to data nodes. Applicable only for the Provisioned IOPS EBS volume type | `number` | `0` | no |
| ebs\_volume\_size | EBS volumes for data storage in GB | `number` | `0` | no |
| ebs\_volume\_type | Storage type of EBS volumes | `string` | `"gp2"` | no |
| elasticsearch\_subdomain\_name | The name of the subdomain for Elasticsearch in the DNS zone (\_e.g.\_ `elasticsearch`, `ui`, `ui-es`, `search-ui`) | `string` | `""` | no |
| elasticsearch\_version | Version of Elasticsearch to deploy (\_e.g.\_ `7.4`, `7.1`, `6.8`, `6.7`, `6.5`, `6.4`, `6.3`, `6.2`, `6.0`, `5.6`, `5.5`, `5.3`, `5.1`, `2.3`, `1.5` | `string` | `"7.4"` | no |
| enabled | Set to false to prevent the module from creating any resources | `bool` | `true` | no |
| encrypt\_at\_rest\_enabled | Whether to enable encryption at rest | `bool` | `true` | no |
| encrypt\_at\_rest\_kms\_key\_id | The KMS key ID to encrypt the Elasticsearch domain with. If not specified, then it defaults to using the AWS/Elasticsearch service KMS key | `string` | `""` | no |
| environment | Environment, e.g. 'prod', 'staging', 'dev', 'pre-prod', 'UAT' | `string` | `""` | no |
| iam\_actions | List of actions to allow for the IAM roles, _e.g._ `es:ESHttpGet`, `es:ESHttpPut`, `es:ESHttpPost` | `list(string)` | `[]` | no |
| iam\_authorizing\_role\_arns | List of IAM role ARNs to permit to assume the Elasticsearch user role | `list(string)` | `[]` | no |
| iam\_role\_arns | List of IAM role ARNs to permit access to the Elasticsearch domain | `list(string)` | `[]` | no |
| iam\_role\_max\_session\_duration | The maximum session duration (in seconds) for the user role. Can have a value from 1 hour to 12 hours | `number` | `3600` | no |
| ingress\_port\_range\_end | End number for allowed port range. (e.g. `443`) | `number` | `65535` | no |
| ingress\_port\_range\_start | Start number for allowed port range. (e.g. `443`) | `number` | `0` | no |
| instance\_count | Number of data nodes in the cluster | `number` | `4` | no |
| instance\_type | Elasticsearch instance type for data nodes in the cluster | `string` | `"t2.small.elasticsearch"` | no |
| kibana\_subdomain\_name | The name of the subdomain for Kibana in the DNS zone (\_e.g.\_ `kibana`, `ui`, `ui-es`, `search-ui`, `kibana.elasticsearch`) | `string` | `"kibana"` | no |
| label\_order | The naming order of the id output and Name tag | `list(string)` | `[]` | no |
| log\_publishing\_application\_cloudwatch\_log\_group\_arn | ARN of the CloudWatch log group to which log for ES\_APPLICATION\_LOGS needs to be published | `string` | `""` | no |
| log\_publishing\_application\_enabled | Specifies whether log publishing option for ES\_APPLICATION\_LOGS is enabled or not | `bool` | `false` | no |
| log\_publishing\_index\_cloudwatch\_log\_group\_arn | ARN of the CloudWatch log group to which log for INDEX\_SLOW\_LOGS needs to be published | `string` | `""` | no |
| log\_publishing\_index\_enabled | Specifies whether log publishing option for INDEX\_SLOW\_LOGS is enabled or not | `bool` | `false` | no |
| log\_publishing\_search\_cloudwatch\_log\_group\_arn | ARN of the CloudWatch log group to which log for SEARCH\_SLOW\_LOGS needs to be published | `string` | `""` | no |
| log\_publishing\_search\_enabled | Specifies whether log publishing option for SEARCH\_SLOW\_LOGS is enabled or not | `bool` | `false` | no |
| name | Solution name, e.g. 'app' or 'jenkins' | `string` | `""` | no |
| namespace | Namespace, which could be your organization name or abbreviation, e.g. 'eg' or 'cp' | `string` | `""` | no |
| node\_to\_node\_encryption\_enabled | Whether to enable node-to-node encryption | `bool` | `false` | no |
| security\_groups | List of security group IDs to be allowed to connect to the cluster | `list(string)` | `[]` | no |
| stage | Stage, e.g. 'prod', 'staging', 'dev', OR 'source', 'build', 'test', 'deploy', 'release' | `string` | `""` | no |
| subnet\_ids | VPC Subnet IDs | `list(string)` | `[]` | no |
| tags | Additional tags (e.g. `map('BusinessUnit','XYZ')` | `map(string)` | `{}` | no |
| vpc\_enabled | Set to false if ES should be deployed outside of VPC. | `bool` | `true` | no |
| vpc\_id | VPC ID | `string` | `null` | no |
| zone\_awareness\_enabled | Enable zone awareness for Elasticsearch cluster | `bool` | `true` | no |

## Outputs

| Name | Description |
|------|-------------|
| domain\_arn | ARN of the Elasticsearch domain |
| domain\_endpoint | Domain-specific endpoint used to submit index, search, and data upload requests |
| domain\_hostname | Elasticsearch domain hostname to submit index, search, and data upload requests |
| domain\_id | Unique identifier for the Elasticsearch domain |
| domain\_name | Name of the Elasticsearch domain |
| elasticsearch\_user\_iam\_role\_arn | The ARN of the IAM role to allow access to Elasticsearch cluster |
| elasticsearch\_user\_iam\_role\_name | The name of the IAM role to allow access to Elasticsearch cluster |
| kibana\_endpoint | Domain-specific endpoint for Kibana without https scheme |
| kibana\_hostname | Kibana hostname |
| security\_group\_id | Security Group ID to control access to the Elasticsearch domain |





## References

For additional context, refer to some of these links.

- [What is Amazon Elasticsearch Service](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/what-is-amazon-elasticsearch-service.html) - Complete description of Amazon Elasticsearch Service
- [Amazon Elasticsearch Service Access Control](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-ac.html) - Describes several ways of controlling access to Elasticsearch domains
- [VPC Support for Amazon Elasticsearch Service Domains](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-vpc.html) - Describes Elasticsearch Service VPC Support and VPC architectures with and without zone awareness
- [Creating and Configuring Amazon Elasticsearch Service Domains](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-createupdatedomains.html) - Provides a complete description on how to create and configure Amazon Elasticsearch Service (Amazon ES) domains
- [Kibana and Logstash](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-kibana.html) - Describes some considerations for using Kibana and Logstash with Amazon Elasticsearch Service
- [Amazon Cognito Authentication for Kibana](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-cognito-auth.html) - Amazon Elasticsearch Service uses Amazon Cognito to offer user name and password protection for Kibana
- [Control Access to Amazon Elasticsearch Service Domain](https://aws.amazon.com/blogs/security/how-to-control-access-to-your-amazon-elasticsearch-service-domain/) - Describes how to Control Access to Amazon Elasticsearch Service Domain
- [elasticsearch_domain](https://www.terraform.io/docs/providers/aws/r/elasticsearch_domain.html) - Terraform reference documentation for the `elasticsearch_domain` resource
- [elasticsearch_domain_policy](https://www.terraform.io/docs/providers/aws/r/elasticsearch_domain_policy.html) - Terraform reference documentation for the `elasticsearch_domain_policy` resource


## Help

**Got a question?** We got answers.

File a GitHub [issue](https://github.com/cloudposse/terraform-aws-elasticsearch/issues), send us an [email][email] or join our [Slack Community][slack].

[![README Commercial Support][readme_commercial_support_img]][readme_commercial_support_link]

## DevOps Accelerator for Startups


We are a [**DevOps Accelerator**][commercial_support]. We'll help you build your cloud infrastructure from the ground up so you can own it. Then we'll show you how to operate it and stick around for as long as you need us.

[![Learn More](https://img.shields.io/badge/learn%20more-success.svg?style=for-the-badge)][commercial_support]

Work directly with our team of DevOps experts via email, slack, and video conferencing.

We deliver 10x the value for a fraction of the cost of a full-time engineer. Our track record is not even funny. If you want things done right and you need it done FAST, then we're your best bet.

- **Reference Architecture.** You'll get everything you need from the ground up built using 100% infrastructure as code.
- **Release Engineering.** You'll have end-to-end CI/CD with unlimited staging environments.
- **Site Reliability Engineering.** You'll have total visibility into your apps and microservices.
- **Security Baseline.** You'll have built-in governance with accountability and audit logs for all changes.
- **GitOps.** You'll be able to operate your infrastructure via Pull Requests.
- **Training.** You'll receive hands-on training so your team can operate what we build.
- **Questions.** You'll have a direct line of communication between our teams via a Shared Slack channel.
- **Troubleshooting.** You'll get help to triage when things aren't working.
- **Code Reviews.** You'll receive constructive feedback on Pull Requests.
- **Bug Fixes.** We'll rapidly work with you to fix any bugs in our projects.

## Slack Community

Join our [Open Source Community][slack] on Slack. It's **FREE** for everyone! Our "SweetOps" community is where you get to talk with others who share a similar vision for how to rollout and manage infrastructure. This is the best place to talk shop, ask questions, solicit feedback, and work together as a community to build totally *sweet* infrastructure.

## Discourse Forums

Participate in our [Discourse Forums][discourse]. Here you'll find answers to commonly asked questions. Most questions will be related to the enormous number of projects we support on our GitHub. Come here to collaborate on answers, find solutions, and get ideas about the products and services we value. It only takes a minute to get started! Just sign in with SSO using your GitHub account.

## Newsletter

Sign up for [our newsletter][newsletter] that covers everything on our technology radar.  Receive updates on what we're up to on GitHub as well as awesome new projects we discover.

## Office Hours

[Join us every Wednesday via Zoom][office_hours] for our weekly "Lunch & Learn" sessions. It's **FREE** for everyone!

[![zoom](https://img.cloudposse.com/fit-in/200x200/https://cloudposse.com/wp-content/uploads/2019/08/Powered-by-Zoom.png")][office_hours]

## Contributing

### Bug Reports & Feature Requests

Please use the [issue tracker](https://github.com/cloudposse/terraform-aws-elasticsearch/issues) to report any bugs or file feature requests.

### Developing

If you are interested in being a contributor and want to get involved in developing this project or [help out](https://cpco.io/help-out) with our other projects, we would love to hear from you! Shoot us an [email][email].

In general, PRs are welcome. We follow the typical "fork-and-pull" Git workflow.

 1. **Fork** the repo on GitHub
 2. **Clone** the project to your own machine
 3. **Commit** changes to your own branch
 4. **Push** your work back up to your fork
 5. Submit a **Pull Request** so that we can review your changes

**NOTE:** Be sure to merge the latest changes from "upstream" before making a pull request!


## Copyright

Copyright © 2017-2020 [Cloud Posse, LLC](https://cpco.io/copyright)



## License

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

See [LICENSE](LICENSE) for full details.

    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      https://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.









## Trademarks

All other trademarks referenced herein are the property of their respective owners.

## About

This project is maintained and funded by [Cloud Posse, LLC][website]. Like it? Please let us know by [leaving a testimonial][testimonial]!

[![Cloud Posse][logo]][website]

We're a [DevOps Professional Services][hire] company based in Los Angeles, CA. We ❤️  [Open Source Software][we_love_open_source].

We offer [paid support][commercial_support] on all of our projects.

Check out [our other projects][github], [follow us on twitter][twitter], [apply for a job][jobs], or [hire us][hire] to help with your cloud strategy and implementation.



### Contributors

|  [![Erik Osterman][osterman_avatar]][osterman_homepage]<br/>[Erik Osterman][osterman_homepage] | [![Andriy Knysh][aknysh_avatar]][aknysh_homepage]<br/>[Andriy Knysh][aknysh_homepage] | [![Igor Rodionov][goruha_avatar]][goruha_homepage]<br/>[Igor Rodionov][goruha_homepage] | [![Sarkis Varozian][sarkis_avatar]][sarkis_homepage]<br/>[Sarkis Varozian][sarkis_homepage] | [![Marcin Brański][3h4x_avatar]][3h4x_homepage]<br/>[Marcin Brański][3h4x_homepage] |
|---|---|---|---|---|

  [osterman_homepage]: https://github.com/osterman
  [osterman_avatar]: https://img.cloudposse.com/150x150/https://github.com/osterman.png
  [aknysh_homepage]: https://github.com/aknysh
  [aknysh_avatar]: https://img.cloudposse.com/150x150/https://github.com/aknysh.png
  [goruha_homepage]: https://github.com/goruha
  [goruha_avatar]: https://img.cloudposse.com/150x150/https://github.com/goruha.png
  [sarkis_homepage]: https://github.com/sarkis
  [sarkis_avatar]: https://img.cloudposse.com/150x150/https://github.com/sarkis.png
  [3h4x_homepage]: https://github.com/3h4x
  [3h4x_avatar]: https://img.cloudposse.com/150x150/https://github.com/3h4x.png

[![README Footer][readme_footer_img]][readme_footer_link]
[![Beacon][beacon]][website]

  [logo]: https://cloudposse.com/logo-300x69.svg
  [docs]: https://cpco.io/docs?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=docs
  [website]: https://cpco.io/homepage?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=website
  [github]: https://cpco.io/github?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=github
  [jobs]: https://cpco.io/jobs?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=jobs
  [hire]: https://cpco.io/hire?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=hire
  [slack]: https://cpco.io/slack?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=slack
  [linkedin]: https://cpco.io/linkedin?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=linkedin
  [twitter]: https://cpco.io/twitter?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=twitter
  [testimonial]: https://cpco.io/leave-testimonial?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=testimonial
  [office_hours]: https://cloudposse.com/office-hours?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=office_hours
  [newsletter]: https://cpco.io/newsletter?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=newsletter
  [discourse]: https://ask.sweetops.com/?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=discourse
  [email]: https://cpco.io/email?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=email
  [commercial_support]: https://cpco.io/commercial-support?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=commercial_support
  [we_love_open_source]: https://cpco.io/we-love-open-source?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=we_love_open_source
  [terraform_modules]: https://cpco.io/terraform-modules?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=terraform_modules
  [readme_header_img]: https://cloudposse.com/readme/header/img
  [readme_header_link]: https://cloudposse.com/readme/header/link?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=readme_header_link
  [readme_footer_img]: https://cloudposse.com/readme/footer/img
  [readme_footer_link]: https://cloudposse.com/readme/footer/link?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=readme_footer_link
  [readme_commercial_support_img]: https://cloudposse.com/readme/commercial-support/img
  [readme_commercial_support_link]: https://cloudposse.com/readme/commercial-support/link?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=readme_commercial_support_link
  [share_twitter]: https://twitter.com/intent/tweet/?text=terraform-aws-elasticsearch&url=https://github.com/cloudposse/terraform-aws-elasticsearch
  [share_linkedin]: https://www.linkedin.com/shareArticle?mini=true&title=terraform-aws-elasticsearch&url=https://github.com/cloudposse/terraform-aws-elasticsearch
  [share_reddit]: https://reddit.com/submit/?url=https://github.com/cloudposse/terraform-aws-elasticsearch
  [share_facebook]: https://facebook.com/sharer/sharer.php?u=https://github.com/cloudposse/terraform-aws-elasticsearch
  [share_googleplus]: https://plus.google.com/share?url=https://github.com/cloudposse/terraform-aws-elasticsearch
  [share_email]: mailto:?subject=terraform-aws-elasticsearch&body=https://github.com/cloudposse/terraform-aws-elasticsearch
  [beacon]: https://ga-beacon.cloudposse.com/UA-76589703-4/cloudposse/terraform-aws-elasticsearch?pixel&cs=github&cm=readme&an=terraform-aws-elasticsearch
