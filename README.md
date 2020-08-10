# cfn-macro-cognito-alias-target

This is an AWS CloudFormation macro to retrieve the alias target for a Cognito
custom hosted UI domain. It can be used when you need to create a Route53 record
set to point to the domain, as the `AWS::Cognito::UserPool::Domain` resource
does not export this value. This macro will retrieve the alias target from the
domain name.

## Creating the Macro

Create the macro by deploying the `template.yml` stack:

    aws [--profile name] cloudformation deploy \
      --capabilities CAPABILITY_IAM \
      --stack-name CognitoAliasTarget \
      --template-file template.yml

Use whatever stack name you wish. The macro name by default will be
`CognitoAliasTarget`, but you can change that by adding `--parameter-overrides
MacroName=MyName`.

## Using the Macro

Here's an example of provisioning an Route53 record set for the custom hosted UI
domain "auth.mydomain.com":

    UserPoolAliasRecord:
      Type: AWS::Route53::RecordSet
      Properties:
        AliasTarget:
          DNSName:
            Fn::Transform:
              Name: CognitoAliasTarget
              Parameters:
                DomainName: auth.mydomain.com
          HostedZoneId: Z2FDTNDATAQYW2
        Name: auth.mydomain.com
        Type: A
