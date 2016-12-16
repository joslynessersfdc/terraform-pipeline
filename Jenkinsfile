node {
   stage 'checkout'
        checkout scm

   stage 'test'
        parallel (
            phase1: { sh "echo p1; sleep 20s; echo phase1" },
            phase2: { sh "echo p2; sleep 40s; echo phase2" }
        )
   stage name: 'build', concurrency: 1
        echo "packer build project.json"

   stage name: 'plan', concurrency: 1
        sh "terraform plan --out plan.tfplan terraform/"

   stage name: 'apply', concurrency: 1
        def deploy_validation = input(
            id: 'apply',
            message: 'apply planned changes',
            type: "boolean")

        sh "terraform apply plan.tfplan"
}
