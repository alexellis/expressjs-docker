pipeline {
	agent none
	stages {
		stage('Descargar Fuentes') {
			agent any
			steps {
				checkout scm
			}
		}
		stage('Compilar y Publicar Docker') {
			agent any
			steps {
				//sh 'docker build -t snahider/expressjs:1.0 .'
				echo 'build'
			}
		}
		stage('Desplegar a Integracion') {
			agent any
			steps {
				sh 'helm upgrade --install expressjs ./chart/expressjs'
			}
		}
		stage('Promocionar a Calidad') {
			when {
				expression {
					timeout(time: 3, unit: 'DAYS') {
						input message: 'Promocionar a Calidad?'
						return true
					}
				}
				beforeAgent true
			}
			agent any
			steps {
				echo 'Promocionando'
			}
		}
		stage('Desplegar a Calidad') {
			agent any
			steps {
				sh 'helm upgrade --install expressjs -f ./chart/expressjs/values-calidad.yaml ./chart/expressjs'
			}
		}
	}
}