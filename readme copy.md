oc new-project tools || true
oc create secret docker-registry ibm-entitlement-key -n tools \
--docker-username=cp \
--docker-password=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJJQk0gTWFya2V0cGxhY2UiLCJpYXQiOjE2NTQ2OTk3MzYsImp0aSI6IjA3Nzc0YjFjYjg2NjQ1MTViYjQ0MzNhZmI0YWZmMzg4In0.i_TWf0DSPWEni9MxEqwzwQrhD2alcht5OncF6IP4DnU \
--docker-server=cp.icr.io

cd multi-tenancy-gitops
GIT_ORG=rxo-gitops-sterling GIT_BRANCH=master ./scripts/set-git-source.sh
git commit -m "Update Git URl and branch references"
git push origin master

oc apply -f setup/ocp4x/  

oc create secret docker-registry ibm-entitlement-key \
  --docker-username=cp \
  --docker-password=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJJQk0gTWFya2V0cGxhY2UiLCJpYXQiOjE2NTQ2OTk3MzYsImp0aSI6IjA3Nzc0YjFjYjg2NjQ1MTViYjQ0MzNhZmI0YWZmMzg4In0.i_TWf0DSPWEni9MxEqwzwQrhD2alcht5OncF6IP4DnU \
  --docker-server=cp.icr.io \
  --namespace=b2bi-prod
