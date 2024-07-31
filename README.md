# azure-terraform-validate-action

A reusable GitHub Action for validating Terraform files in a GitHub repository. Supports both being called from a Pull Request context to generate a terraform plan, as well as in "drift detection" mode by a cron schedule, and will post to a slack webhook room that drift has been detected. 

Can be called like this: 
```yaml
- name: Terraform Validate
  uses: kymidd/azure-terraform-validate-action@master
  with:
    SSH_KEY: ${{ secrets.SSH_KEY }}
    location: ${{ env.location }}
    solution_name: ${{ env.solution_name }}
    terraform_version: ${{ env.tf_version }}
    az_tenant_id: ${{ env.az_tenant_id }}
    az_client_id: ${{ env.az_client_id }}
    az_subscription_id: ${{ env.az_subscription_id }}
    tf_storage_resource_group_name: ${{ env.tf_storage_resource_group_name }}
    tf_storage_account_name: ${{ env.tf_storage_account_name }}
    tf_storage_container_name: ${{ env.tf_storage_container_name }}
    tf_state_filename: ${{ env.tf_state_filename }}
    drift_slack_webhook_url: "https://hooks.slack.com/services/xxxxx/yyyyy"
```

If you need to pass additional terraform command, you can encode them like this: 
```yaml
- name: Populate Env Vars
  run: |
    # Set terraform secret vars string
    tf_plan_vars=-var="\"secret_key=${{ secrets.SECRET_KEY }}\" -var=\"secret_key2=${{ secrets.SECRET_KEY2 }}\""

    # Write value to GitHub Action env
    echo "tf_plan_vars=$tf_plan_vars" | tee -a $GITHUB_ENV

    # Mask values
    echo "::add-mask::$tf_plan_vars"

# Call the Action as a step in your workflow
- name: Terraform Validate
  uses: kymidd/azure-terraform-validate-action@master
  with:
    SSH_KEY: ${{ secrets.SSH_KEY }}
    location: ${{ env.location }}
    solution_name: ${{ env.solution_name }}
    terraform_version: ${{ env.tf_version }}
    az_tenant_id: ${{ env.az_tenant_id }}
    az_client_id: ${{ env.az_client_id }}
    az_subscription_id: ${{ env.az_subscription_id }}
    tf_storage_resource_group_name: ${{ env.tf_storage_resource_group_name }}
    tf_storage_account_name: ${{ env.tf_storage_account_name }}
    tf_storage_container_name: ${{ env.tf_storage_container_name }}
    tf_state_filename: ${{ env.tf_state_filename }}
    tf_plan_vars: ${{ env.tf_plan_vars }}
    drift_slack_webhook_url: "https://hooks.slack.com/services/xxxxx/yyyyy"
```