# ACM


- Install cert-manager
    - Create LetsEncrypt Cluster Issuer
        - Create Policy
        ```
        aws iam create-policy \
          --policy-name cert-manager-dns01-route53 \
          --description "This policy allows cert-manager to manage ACME DNS01 records in Route53 hosted zones. See https://cert-manager.io/docs/configuration/acme/dns01/route53" \
          --policy-document '{
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
          }'
        ```
        - Create role
        ```
        AWS_ACCOUNT_ID=$(aws sts get-caller-identity --query "Account" --output text)
        aws iam create-role \
          --role-name cert-manager-dns01-role \
          --assume-role-policy-document '{
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Principal": {
                        "Service": "pods.eks.amazonaws.com"
                    },
                    "Action": [
                        "sts:AssumeRole",
                        "sts:TagSession"
                    ]
                }
            ]
          }'
        
        aws iam attach-role-policy \
          --role-name cert-manager-dns01-role \
          --policy-arn arn:aws:iam::${AWS_ACCOUNT_ID}:policy/cert-manager-dns01-route53
        ```
      - Create cert-manager pod identity with the above role
      - Restart cert-manager to get new credentials
- Install nginx-controller
    -   helm install my-nginx ingress-nginx/ingress-nginx --namespace nginx-ingress --values nginx-ingress.yaml
    
- Create Cert-Issuer for LetsEncrypt
- Create Deployment, Svc and Ingress

- 
  
