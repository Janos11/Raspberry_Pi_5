# Raspberry Pi 5 Journey

Welcome to my Raspberry Pi 5 project notebook.  
This repository documents my learning, experiments, and personal projects with Raspberry Pi OS, Docker, and various services.  
It showcases my ability to self-start, learn new technologies, and organize a growing homelab.

---

## Table of Contents
- [Latest Updates](#latest-updates)
- [Docker Projects](#docker-projects)
- [Bare Metal Services](#bare-metal-services)
- [Monitoring & Tools](#monitoring--tools)
- [Learning Notebook](#learning-notebook)
- [Contributors](#contributors)

---

## Latest Updates
- Installed Portainer for container management.
- Migrated Samba from bare metal to Docker for portability and easier management.
- Installed Docker
- Configured SSH and optimized Pi OS for development.

---

## Docker Projects
- Containerized services:
  - [Samba](notebooks/samba_setup_pi.md)
  - Portainer
  - Home Assistant (future)
- All configurations stored with Docker Compose for reproducibility.
- [See Docker setup details](notebooks/docker_setup_rpi_5.md)

---

## Bare Metal Services
- Initial experiments with:
  - SSH
  - git [git_cheat_sheet.md](https://github.com/Janos11/Robot_Web_Controller/blob/master/git_cheat_sheet.md)
  - System monitoring tools [raspberry_pi_linux_commands.md](notebooks/raspberry_pi_linux_commands.md)
- Learning how services interact natively on Raspberry Pi OS.

---

## Monitoring & Tools
- Commands and tools used:
  - `htop`, `top`, `df -h`, `docker stats`
  - Temperature monitoring: `vcgencmd measure_temp`
- Tracking system usage and container health.

---

## Learning Notebook
- Structured as a timeline of experiments, lessons, and configs.
- Each service or experiment has its own file in `notebooks/` for easy reference.
- Example files:
  - `notebooks/samba.md`
  - [raspberry_pi_linux_commands.md](https://github.com/Janos11/Raspberry_Pi_5/blob/main/notebooks/raspberry_pi_linux_commands.md)
  - `notebooks/docker-setup.md`
  - `notebooks/monitoring.md`


---

## Contributors

<table style="font-family: Arial, sans-serif; line-height: 1.6;">
  <tr>
    <td><strong>János Rostás</strong></td>
    <td>
      👨‍💻 Electronic & Computer Engineer<br>
      🛠️ Tinkerer with a Purpose<br>
      🐳 Docker Enthusiast<br>
      🌐 <a href="https://janosrostas.co.uk" target="_blank">janosrostas.co.uk</a><br>
      🔗 <a href="https://www.linkedin.com/in/janos-rostas/" target="_blank">LinkedIn</a>
    </td>
  </tr>
  <tr>
    <td><strong>ChatGPT</strong></td>
    <td>
      🤖 AI Pair Programmer by OpenAI<br>
      💡 Supports brainstorming, prototyping, and debugging<br>
      📚 Backed by years of programming knowledge and best practices
    </td>
  </tr>
</table>
  
