# Markdown Template Action

> Quickly build markdown summaries for your workflows.
> - Supports environment variable injection
> - Allows you to write summaries in YAML
> - Much easier deal with comparing with inline bash summaries.

## Example usage
The following is an extract from our [`example-template.yml`](.github/workflows/example-template.yml) workflow.
You can see it in action by going to the 
"[`Actions`](https://github.com/WcAServices/markdown-template-action/actions/workflows/example-template.yml)"
tab in this repo.
````yaml
steps:
  - uses: WcAServices/markdown-template-action@v1
    with:
      # These will be injected into the below template, in addtion to GitHub's standard variables.
      # You can perform string operations here as well.
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

### Result
Once your workflow has completed, you'll receive a nice summary at the bottom of the page with your markdown template. Using the workflow above, you should end up with something like the following;
[![image](https://user-images.githubusercontent.com/4034561/215385906-e03c34d9-25ef-4d92-a377-001ec6ef37c6.png)](https://github.com/WcAServices/markdown-template-action/actions/runs/4040501150/attempts/1#summary-10964953140)


## Reasoning
When building step summaries, you tend to get stuck with GitHub's suggested apporoach where you simply echo into the step summary variable.
This can quickly get very verbose when you get into slightly more advanced markdown formatting:
> - A great alternative to the following templating workflow; 👎
>    ````bash
>    echo "# Tests are failing"                                         >> $GITHUB_STEP_SUMMARY
>    echo "> Go blame $GITHUB_ACTOR who pushed to $GITHUB_BASE_REF"     >> $GITHUB_STEP_SUMMARY
>    echo ""                                                            >> $GITHUB_STEP_SUMMARY
>    echo '```'                                                         >> $GITHUB_STEP_SUMMARY
>    echo "$TEST_SUMMARY_RESULTS"                                       >> $GITHUB_STEP_SUMMARY
>    echo '```'                                                         >> $GITHUB_STEP_SUMMARY
>    ````
