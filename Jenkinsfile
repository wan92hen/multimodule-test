node ("${compiler}")
{
  stage('Checkout')
		{

					checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/wangzhen-fit2cloud/multimodule-test.git']]])
					echo ">>>>>>>>>>>>>>>>>>>>>>>>>>>Checkout Success<<<<<<<<<<<<<<<<<<<<<<<<<<"

		}
  stage('Build')
		{

				sh '''mvn clean package'''

		}
  stage('F2C CodeDeploy') 
        {
            sh '''
                # CodeDeploy Zip文件设置
                export F2C_ENDPOINT=${F2C_ENDPOINT}
                export F2C_ACCESS_KEY=${F2C_ACCESS_KEY}
                export F2C_SECRET_KEY=${F2C_SECRET_KEY}
        
                export REPO_NAME="${REPO_NAME}"
                export APP_NAME="${APP_NAME}"
                export APP_VERSION="${VERSION}"
                export APP_DESC="null"

                # Zip文件上传设置
                export GROUP_ID="${GROUP_ID}"
                export ARTIFACT_ID="${ARTIFACT_ID}"
                export ARTIFACT_VERSION="${VERSION}"
                export APP_ADDRESS="\${GROUP_ID//.//}/${ARTIFACT_ID}/${VERSION}/${ARTIFACT_ID}-${VERSION}.zip"
               
                # 代码部署设置
                export AUTO_DEPLOY=${AUTO_DEPLOY}
                export CLUSTER_NAME=${CLUSTER_NAME}
                export CLUSTER_ROLE_NAME=${CLUSTER_ROLE_NAME}
                export SERVER_ID=${SERVER_ID}
                export DEPLOY_STRATEGY=${DEPLOY_STRATEGY}
                #export CONTACT_GROUP_ID="null"

                # 等待部署成功设置
                export WAITFOR_COMPLETION=true
                export TIMEOUT_SEC=600
                export INTERNAL_SEC=15
                
                #/bin/bash /opt/f2c-codedeploy.sh AddAppVersion   
                #/bin/bash /opt/f2c-codedeploy.sh AutoDeploy
				for i in \$(ls -d */)
				do
				  export ARTIFACT_ID=\${i%\\/}
				  export APP_NAME=\${i%\\/}
				  echo \$APP_NAME
				  done
            '''
        }
}