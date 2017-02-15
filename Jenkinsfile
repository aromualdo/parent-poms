node("java:8"){
  def git = tool("git")
  def mvn = tool("maven") + "/bin/mvn -B"

  checkout scm

  sh "${git} config user.email engineering+jenkins2@mainstreethub.com"
  sh "${git} config user.name jenkins"
  withCredentials([[$class: "UsernamePasswordMultiBinding",
                    credentialsId: "github-http",
                    usernameVariable: "GIT_USERNAME",
                    passwordVariable: "GIT_PASSWORD"]]) {
    def username = URLEncoder.encode("${env.GIT_USERNAME}", "UTF-8")
    def password = URLEncoder.encode("${env.GIT_PASSWORD}", "UTF-8")

    writeFile(file: "${env.HOME}/.git-credentials",
        text: "https://${username}:${password}@github.com")
    sh "${git} config credential.helper store --file=${env.HOME}/.git-credentials"
  }

  // Write the gpg.conf to the container
  def gpg = "keyserver hkp://keys.gnupg.net"
  writeFile(file: ".gnupg/gpg.conf",
      text: gpg)


  withCredentials([file(credentialsId: 'pubring.gpg', variable: 'TOKEN')]) {
    sh "cp ${TOKEN} .gnupg/pubring.gpg"
  }

  sh "ls -l"

//  writeFile(file: ".gnupg/secring.gpg",
//      text: gpg)
//
//  writeFile(file: ".gnupg/trustdb.gpg",
//      text: gpg)
//
//  stage("Compile") {
//    sh "${mvn} -Popen-source -Dresume=false -DdryRun=true -Dmaven.javadoc.skip=true -Darguments=\"-Popen-source -DskipTests=true -DskipITs=true -Dmaven.javadoc.skip=true\" release:clean release:prepare"
//  }

//  stage("Test") {
//    sh "${mvn} -Popen-source -Dresume=false -Dmaven.javadoc.skip=true -Darguments=\"-Popen-source -DskipTests=true -DskipITs=true -Dmaven.javadoc.skip=true\" release:clean release:prepare release:perform"
//  }

}