---
layout: post
title: Envy commands and troubleshooting ! 
---

[comment]:![_config.yml]({{ site.baseurl }}/images/config.png)

`kubectl get pods`

`kubectl get svs`


[commet]: TODO Make it better
Madan Kapoor  5:25 PM
http://localenv.ninja:30639/

Madan Kapoor  7:43 PM
Hi in msfteams this script is used to inject config into code during deploy
7:44
set -eu
UI_COMPONENT_NAMESPACE=app
UI_COMPONENT_NAME=msft-teams-ui
UI_WEB_HOST=ui.schoologystg.com
# Download artifact from artifactory
artifact=https://schoology.jfrog.io/schoology/npm-local/msft-teams/-/msft-teams-${bamboo_deploy_release}.tgz
creds=$(grep _auth ~/.npmrc | cut -d'=' -f2- | sed -e "s/ //" | base64 --decode)
echo Downloading $artifact
curl -u $creds -O $artifact
tar xvfz msft-teams-${bamboo_deploy_release}.tgz
# Assume role to be able to write to s3
IAM_ROLE=${IAM_ROLE:-arn:aws:iam::${bamboo_inject_aws_account_id}:role/${bamboo_inject_environment_short_name}-deploy-ui-role}
ASSUME_ROLE_JSON=$(aws sts assume-role --role-arn "${IAM_ROLE}" --role-session-name ui-deploy)
export AWS_ACCESS_KEY_ID=$(echo "${ASSUME_ROLE_JSON}" | jq -er .Credentials.AccessKeyId)
export AWS_SECRET_ACCESS_KEY=$(echo "${ASSUME_ROLE_JSON}" | jq -er .Credentials.SecretAccessKey)
export AWS_SESSION_TOKEN=$(echo "${ASSUME_ROLE_JSON}" | jq -er .Credentials.SessionToken)
# Get the client ID for this environment and inject it into the script
msft_client_id=$(curl -s "http://${bamboo_inject_consul_agent_host}/v1/kv/app-platform-apps/msft-teams/msft-client-id" | jq -r .[0].Value  | base64 -d)
if [ -z "$msft_client_id" ]; then
	echo 'Unable to retrieve MSFT Client ID'
    exit 1
fi
grep -lr PLACEHOLDER-MSFT-CLIENT-ID-000000000 . | xargs sed -ie "s/PLACEHOLDER-MSFT-CLIENT-ID-000000000/${msft_client_id}/g"
# Copy files to S3
src=package/build
target="s3://schoology${bamboo_inject_environment_short_name}-ui/app/msft-teams-ui/${bamboo_deploy_release}/"
# Also copy to a "Latest" location temporarily since the app currently doesn't support
# non-root deployment so we need cloudfront to hava a static path to serve files
target_latest="s3://schoology${bamboo_inject_environment_short_name}-ui/app/msft-teams-ui/latest/"
echo Copying to S3
aws s3 cp --recursive $src $target --recursive --cache-control max-age=3600
aws s3 cp --recursive $src $target_latest --recursive --cache-control max-age=3600
echo "saving version info in config repo"
git clone ssh://git@bitbucket.schoologize.com:7999/sys/config.git
cd config
mkdir -p registry/${UI_COMPONENT_NAMESPACE}/${UI_COMPONENT_NAME}
MESSAGE="${UI_COMPONENT_NAMESPACE}/${UI_COMPONENT_NAME} v${bamboo_deploy_release} deployed to ${bamboo_inject_environment_name} on `date -u`"
cat > registry/${UI_COMPONENT_NAMESPACE}/${UI_COMPONENT_NAME}/${bamboo_inject_environment_name}.yaml << YAML
# ${MESSAGE}
registry:
  ${UI_COMPONENT_NAMESPACE}:
    ${UI_COMPONENT_NAME}:
      bundle_js_url: https://${UI_WEB_HOST}/${UI_COMPONENT_NAMESPACE}/${UI_COMPONENT_NAME}/${bamboo_deploy_release}
YAML
git add registry/${UI_COMPONENT_NAMESPACE}/${UI_COMPONENT_NAME}/${bamboo_inject_environment_name}.yaml
git commit -m "${MESSAGE}"
git push
7:44
https://ci.schoologize.com/deploy/config/configureEnvironmentTasks.action?environmentId=375521281

Madan Kapoor  8:15 PM
http://stg-consul-server.sgyint.com/ui/awsstaging/kv/app-platform-apps/msft-teams/