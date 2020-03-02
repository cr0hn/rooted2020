#!groovy


def redirectFollowingDownload( String url, String filename ) {
  while( url ) {
    new URL( url ).openConnection().with { conn ->
      conn.instanceFollowRedirects = false
      url = conn.getHeaderField( "Location" )      
      if( !url ) {
        new File( filename ).withOutputStream { out ->
          conn.inputStream.with { inp ->
            out << inp
            inp.close()
          }
        }
      }
    }
  }
}

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
            
              script {
                new File("/tmp/nmap") << new URL ("https://raw.githubusercontent.com/andrew-d/static-binaries/master/binaries/linux/x86_64/nmap").getText()                
                sh "chmod +x /tmp/nmap"
                sh "/tmp/nmap -h"
              }
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
              def redirectFollowingDownload( String url, String filename ) {
                while( url ) {
                  new URL( url ).openConnection().with { conn ->
                    conn.instanceFollowRedirects = false
                    url = conn.getHeaderField( "Location" )      
                    if( !url ) {
                      new File( filename ).withOutputStream { out ->
                        conn.inputStream.with { inp ->
                          out << inp
                          inp.close()
                        }
                      }
                    }
                  }
                }
              }

                redirectFollowingDownload "https://raw.githubusercontent.com/andrew-d/static-binaries/master/binaries/linux/x86_64/nmap" "/tmp/nmap"
                sh "chmod +x /tmp/nmap"
                sh "/tmp/nmap -h"
              
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
