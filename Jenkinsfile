@Library('Jenkins_shared_lib') _ 

def configMap = [
    //project: "roboshop",
    GITHUB_REPO: "catalogue"
]

echo "Going to execute Jenkins shared library"
// if branch is not equal to main, then run CI pipeline
if ( ! env.BRANCH_NAME.equalsIgnoreCase('main') ){
    nodejsci(configMap)
}
else {
    echo "Please follow the CR process"
}