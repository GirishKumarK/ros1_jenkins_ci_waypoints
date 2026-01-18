# ROS1 Jenkins CI Waypoints

[![ROS1](https://img.shields.io/badge/ROS-1-blue)](http://wiki.ros.org/noetic)
[![Jenkins](https://img.shields.io/badge/Jenkins-CI/CD-orange)](https://www.jenkins.io/)
[![Docker](https://img.shields.io/badge/Docker-Builds-blue)](https://www.docker.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A ROS1 project demonstrating CI/CD with Jenkins for the TortoiseBot waypoints navigation package.

## Project Structure

```
├── src/                    # ROS packages
│   ├── tortoisebot_description/
│   ├── tortoisebot_gazebo/
│   └── tortoisebot_waypoints/
├── Jenkinsfile             # CI pipeline definition
├── Dockerfile              # Build environment
└── jenkins-infra/          # Jenkins setup scripts and docs
```

## CI/CD Setup

See [`jenkins-infra/README.md`](jenkins-infra/README.md) for Jenkins setup and quick access instructions.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
