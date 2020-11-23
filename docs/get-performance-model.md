# How to get performance model

You can run testing framework upon your testcase to get its performance model as the testing result. 

## Step 1. Setup basic components in your aws account.

please follow [setup-basic-components-in-aws-account](setup-basic-components-in-aws-account.md)


## Step 2. Build AWS Otel Collector Package. 

1. Checkout AWS Otel Collector in the same folder of the testing framework if you haven't done that.

```
git clone git@github.com:aws-observability/aws-otel-collector.git
```

2. Build RPM which will be used to install AWS Otel Collector in the performance test.

```
cd aws-otel-collector && make package-rpm
```

## Step 3. Run the performance test and get the performance model.

You are going to run three rounds of the test, each takes one hour, performance model is presented as a file `performance_model.json` after each round.


1. Run on rate 100 tps

```shell
cd aws-otel-test-framework/terraform/performance
terraform init
terraform apply -var="data_rate=100" -var="testcase=../testcases/{{testcase name}}" -var="install_package_source=local" -var-file="../testcases/{{testcase name}}/parameters.tfvars"
terraform destroy
cat performance_model.json
```

2. Run on rate 1000 tps

```shell
cd aws-otel-test-framework/terraform/performance
terraform init
terraform apply -var="data_rate=1000" -var="testcase=../testcases/{{testcase name}}" -var="install_package_source=local" -var-file="../testcases/{{testcase name}}/parameters.tfvars"
terraform destroy
cat performance_model.json
```

3. Run on rate 10000 tps

```
cd aws-otel-test-framework/terraform/performance
terraform init
terraform apply -var="data_rate=1000" -var="testcase=../testcases/{{testcase name}}" -var="install_package_source=local" -var-file="../testcases/{{testcase name}}/parameters.tfvars"
terraform destroy
cat performance_model.json
```

4. Put the contents of the `performance_model.json` into the pr you are going to create in the [AWS Otel Collector Repo](https://github.com/aws-observability/aws-otel-collector). 
