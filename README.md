# Markdown Template Action
> Quickly build inline markdown step summaries for your workflows.
> - Supports environment variable injection ✅
>    ````yaml
>       template: |
>         # Tests are failing!
>         > Go blame $GITHUB_ACTOR who pushed to $GITHUB_BASE_REF
>    
>         ## Test Report
>         ```
>           $TEST_SUMMARY_RESULTS
>         ```
>    ````
> - A great alternative to the following templating workflow; 👎
>    ````bash
>    echo "# Tests are failing"                                         >> $GITHUB_STEP_SUMMARY
>    echo "> Go blame $GITHUB_ACTOR who pushed to $GITHUB_BASE_REF"     >> $GITHUB_STEP_SUMMARY
>    echo ""                                                            >> $GITHUB_STEP_SUMMARY
>    echo '```'                                                         >> $GITHUB_STEP_SUMMARY
>    echo "$TEST_SUMMARY_RESULTS"                                       >> $GITHUB_STEP_SUMMARY
>    echo '```'                                                         >> $GITHUB_STEP_SUMMARY
>    ````

## Using the action
The following is an extract from our [`example-template.yml`](.github/workflows/example-template.yml) workflow.
You can see it in action by going to the 
"[`Actions`](https://github.com/WcAServices/markdown-template-action/actions/workflows/example-template.yml)"
tab in this repo.
````yaml
steps:
  - name: 'Template without variables'
    uses: WcAServices/markdown-template-action@main
    with:
      # These will be injected into the below template.
      variables: >-
        GREETING="World"
        COMMIT_SHA="${{ github.sha }}"
        
      # The following language=".." comment
      # enables syntax highlighting in JetBrains IDEs
        
      # language="markdown"
      template: |
        # Hello, $GREETING
        > ### You're on commit `$COMMIT_SHA`            
          
        ## Generally available environment variables should also work;
        ##### Repository
        ```yaml
        Github Ref: $GITHUB_REF
        Commit author: $GITHUB_ACTOR
        Job: $GITHUB_JOB
        ```
````