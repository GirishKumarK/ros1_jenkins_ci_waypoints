pipeline {
  agent any

  environment {
    DOCKER_IMAGE = 'legalaspro/tortoisebot-noetic-waypoints-ci'
    GAZEBO_STARTUP_TIMEOUT = '60'
  }

  stages {
    stage('Build Docker Image') {
      steps {
        echo 'Building Docker image...'
        sh 'docker build -t ${DOCKER_IMAGE}:latest .'
      }
    }

    stage('Run Tests (Headless)') {
      steps {
        echo 'Starting Gazebo headless + running tests...'
        sh '''
          docker run --rm ${DOCKER_IMAGE}:latest bash -lc '
            set -e
            source /opt/ros/noetic/setup.bash
            source /catkin_ws/devel/setup.bash

            cleanup() {
              echo "Cleaning up..."
              [ -n "${LAUNCH_PID:-}" ] && kill -INT "$LAUNCH_PID" >/dev/null 2>&1 || true
              [ -n "${LAUNCH_PID:-}" ] && wait "$LAUNCH_PID" >/dev/null 2>&1 || true
              echo "Cleanup done."
            }
            trap cleanup EXIT

            echo "Launching Gazebo (headless)..."
            roslaunch tortoisebot_gazebo tortoisebot_playground.launch gui:=false headless:=true >/dev/null 2>&1 &
            LAUNCH_PID=$!
            echo "Gazebo launch started (PID: $LAUNCH_PID)"

            echo "Waiting for /odom (timeout: ${GAZEBO_STARTUP_TIMEOUT}s)..."
            timeout ${GAZEBO_STARTUP_TIMEOUT} bash -lc "
              until rostopic echo -n 1 /odom >/dev/null 2>&1; do
                sleep 1
              done
            " || { echo "ERROR: Timeout waiting for /odom"; exit 1; }

            echo "Simulation ready (/odom is publishing)."
            echo "Running waypoints rostest..."
            rostest tortoisebot_waypoints waypoints_test.test --reuse-master

            echo "Tests finished."
          '
        '''
      }
    }
  }

  post {
    success { echo 'Pipeline completed successfully!' }
    failure { echo 'Pipeline failed!' }
  }
}
