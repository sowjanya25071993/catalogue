#!groovy
@Library('roboshop-shared-library') _
def configMap=[
    application="nodejsVM",
    component="catalogue"
]
if (!env.BRANCH_NAME.equalsIgnoreCase('main')){
    pipelineDecision.decidePipeline(configMap)
}
else{
    echo "this is production , deal with cr process"
}