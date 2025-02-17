# ACM


- Install cert-manager
    - Create LetsEncrypt Cluster Issuer
        - Create Policy
        ```
            aws iam create-policy \
                 --policy-name cert-manager-acme-dns01-route53 \
                 --description "This policy allows cert-manager to manage ACME DNS01 records in Route53 hosted zones. See https://cert-manager.io/docs/configuration/acme/dns01/route53" \
                 --policy-document file:///dev/stdin <<EOF
            {
              "Version": "2012-10-17",
              "Statement": [
                {
                  "Effect": "Allow",
                  "Action": "route53:GetChange",
                  "Resource": "arn:aws:route53:::change/*"
                },
                {
                  "Effect": "Allow",
                  "Action": [
                    "route53:ChangeResourceRecordSets",
                    "route53:ListResourceRecordSets"
                  ],
                  "Resource": "arn:aws:route53:::hostedzone/*"
                },
                {
                  "Effect": "Allow",
                  "Action": "route53:ListHostedZonesByName",
                  "Resource": "*"
                }
              ]
            }
            EOF        
        ```
- Install nginx-controller
    -   helm install my-nginx ingress-nginx/ingress-nginx --namespace nginx-ingress --values nginx-ingress.yaml
- 
  
