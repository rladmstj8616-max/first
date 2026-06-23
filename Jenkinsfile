Pipeline {
    agent any

    //environment {
        //필요하다면 Python 환경 경로 설정
        //PYTHONPATH = env.WORKSPACE
    //}

    stages{
        stage('Checkout') {
            steps {
                // Git repository에서 코드 체크아웃
                checkout scm
            }
        }

        stage('Set Up Python Enviroment') {
            steps {
                //Windows 환경에 맞춘 Python 가상환경 생성 및 의존성 설치
                bat '''
                    python -m venv venv
                    call venv||Scripts||activate
                    python -m pip install --upgrade pip
                    python -m pip install -r requirements.txt
                '''
            }
        }
        stage('Run pytest') {
            steps {
                //pytest 실행하여 테스트 수행, JUnit XML 리포트 생성
                bat '''
                    call venv||Scipts||activate
                    python -m pytest tests/ --junitxml = pytest-report.xml
                '''
            }
        }
    }
    post {
        always {
            //JUnit 테스트 결과 보고서 Jenkins에 게시 (Post-buile 설정 자동화)
            junit 'pytest-report.xml'
        }
        success (
            echo '테스트 자동화가 성공적으로 완료되었습니다!'

        )
        failure {
            echo '테스트 자동화 중 일부 테스트가 실패했습니다. 리포트를 확인해주세요'
        }
    }
    }
}