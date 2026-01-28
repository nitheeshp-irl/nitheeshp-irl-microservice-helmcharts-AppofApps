CLUSTER_ARN=$(aws eks describe-cluster \
  --name argocd-poc \
  --query 'cluster.arn' \
  --region us-east-2 \
  --output text)


export ARGOCD_SERVER=$(aws eks describe-capability \
  --cluster-name argocd-poc \
  --capability-name argocd-poc-argocd \
  --query 'capability.configuration.argoCd.serverUrl' \
  --output text \
  --region us-east-2 | sed 's|^https://||')

# Register the cluster using Argo CD CLI
argocd cluster add $CLUSTER_ARN \
  --aws-cluster-name $CLUSTER_ARN \
  --name in-cluster \
  --region us-east-2 \
  --project default

  https://docs.aws.amazon.com/eks/latest/userguide/argocd-concepts.html