node('master') {
    try {
        stage('build') {
            // Checkout the app at the given commit sha from the webhook
            checkout scm

            // Install dependencies, create a new .env file and generate a new key, just for testing
            bat 'docker run --rm --volume %cd%:/git/TestProjectCloverLaravel composer install --no-ansi'
            bat 'cp .env.example .env'
            bat 'docker run --rm --name key-generate-container -v  %cd%:/git/TestProjectCloverLaravel -w /usr/src/app php:7.2-cli php artisan key:generate'            

            // Run any static asset building, if needed
            // sh "npm install && gulp --production"
        }

        stage('test') {
            // Run any testing suites
            bat 'docker run --rm --name phpunit-container -v  %cd%:/git/TestProjectCloverLaravel -w /usr/src/app php:7.2-cli php ./vendor/bin/phpunit'
        }

        stage('deploy') {
            // If we had ansible installed on the server, setup to run an ansible playbook
            // sh 'ansible-playbook -i ./ansible/hosts ./ansible/deploy.yml'
            // sh "echo 'WE ARE DEPLOYING'"
        }
    } catch(error) {
        throw error
    } finally {
        // Any cleanup operations needed, whether we hit an error or not
    }

}
