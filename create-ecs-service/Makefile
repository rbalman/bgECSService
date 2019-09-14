build-binary:
	cd src && go mod download && \
	GOOS=linux GOARCH=amd64 go build -o main main.go
package:
	sam package --template-file saml.yaml --output-template-file packaged.yaml --s3-bucket $(SAM_SOURCE_BUCKET)
deploy: build-binary package
	sam deploy --template-file packaged.yaml --stack-name $(STACK_NAME) --capabilities CAPABILITY_IAM --parameter-overrides EnvironmentName=$(ENV_LABEL) SubSystem=$(SUB_SYSTEM)
delete:
	aws cloudformation delete-stack --stack-name $(STACK_NAME)

test-local: build-binary
	sam local invoke --event events/create-request.json -t saml.yaml
