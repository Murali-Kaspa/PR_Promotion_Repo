pipeline {
    agent any

    stages {

        stage('Checkout PR') {
            steps {
                checkout scm
            }
        }

        stage('Branch Promotion Validation') {
            steps {
                script {
                    def sourceBranch = env.CHANGE_BRANCH
                    def targetBranch = env.CHANGE_TARGET

                    echo "Source Branch: ${sourceBranch}"
                    echo "Target Branch: ${targetBranch}"

                    if (!sourceBranch || !targetBranch) {
                        error("This pipeline must be triggered by a Pull Request.")
                    }

                    def isValidPromotion = false

                    if (sourceBranch.startsWith('feature/') && targetBranch == 'DEV') {
                        isValidPromotion = true
                    } else if (sourceBranch == 'DEV' && targetBranch == 'SIT') {
                        isValidPromotion = true
                    } else if (sourceBranch == 'SAT' && targetBranch == 'UAT') {
                        isValidPromotion = true
                    } else if (sourceBranch == 'UAT' && targetBranch == 'SVP') {
                        isValidPromotion = true
                    }

                    if (!isValidPromotion) {
                        error("""
❌ Invalid Branch Promotion

Source Branch: ${sourceBranch}
Target Branch: ${targetBranch}

Allowed promotion flow:

feature/* → DEV
DEV       → SIT
SIT       → UAT
UAT       → SVP

Please promote the changes through the correct branch sequence.
""")
                    }

                    echo "✅ Valid branch promotion: ${sourceBranch} → ${targetBranch}"
                }
            }
        }
    }
}
