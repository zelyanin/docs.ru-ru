---
title: Модернизация жизненного цикла приложений с помощью конвейеров непрерывной интеграции и непрерывного развертывания и средств DevOps в облаке
description: Модернизировать существующих приложений .NET с помощью Azure Cloud and Windows Containers | Модернизировать жизненный цикл приложения с помощью конвейеров CI/CD и средств DevOps в облаке
ms.date: 04/30/2018
ms.openlocfilehash: d1aa2e156e87cafe99fb994233786f67bf7a81a1
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72396257"
---
# <a name="modernize-your-apps-lifecycle-with-cicd-pipelines-and-devops-tools-in-the-cloud"></a>Модернизация жизненного цикла приложений с помощью конвейеров непрерывной интеграции и непрерывного развертывания и средств DevOps в облаке

Современные компании нуждаются в новшестве на быстром темпе, чтобы быть конкурентоспособным в Marketplace. Для предоставления высококачественных современных приложений требуются средства и процессы DevOps, которые важны для реализации этого постоянного цикла инноваций. С помощью правильных средств DevOps разработчики могут упростить непрерывное развертывание и быстро получать инновационные приложения в руки пользователей.

Несмотря на то что непрерывная интеграция и рекомендации по развертыванию хорошо установлены, введение контейнеров включает в себя новые соображения, особенно при работе с многоконтейнерными приложениями.

Azure DevOps Services поддерживает непрерывную интеграцию и развертывание приложений с несколькими контейнерами в различных средах с помощью официальных задач развертывания Azure DevOps Services.

- [Развертывание в Веб-приложение для контейнеров Azure](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/deploy-docker-webapp?tabs=dotnet-core)

- [Развертывание в службе Kubernetes Azure](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/deploy-aks?tabs=dotnet-core)

Но вы также можете выполнить развертывание в [DOCKER Swarm](https://blogs.msdn.microsoft.com/jcorioland/2016/11/29/full-ci-cd-pipeline-to-deploy-multi-containers-application-on-azure-container-service-docker-swarm-using-visual-studio-team-services/) или DC/OS с помощью Azure DevOps Services задач на основе сценариев.

Чтобы продолжить обеспечение гибкости развертывания, эти средства предоставляют отличные возможности разработки и тестирования для рабочих нагрузок контейнеров, а также решения для разработки и непрерывной интеграции и компакт-дисков.

На рис. 4-12 показан конвейер непрерывного развертывания, который развертывается в кластере Kubernetes в службе контейнеров Azure.

![Снимок экрана Azure DevOps Services развертывании в кластере Kubernetes.](./media/life-cycle-ci-cd-pipelines-devops-tools/deploy-mvc-app-container-kubernetes.png)

**Рис. 4-12.** Azure DevOps Services конвейера непрерывного развертывания, развертывание в кластере Kubernetes

>[!div class="step-by-step"]
>[Назад](modernize-your-apps-with-monitoring-and-telemetry.md)
>[Вперед](migrate-to-hybrid-cloud-scenarios.md)
