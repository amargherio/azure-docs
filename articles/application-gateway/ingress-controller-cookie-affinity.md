---
title: Enable cookie based affinity with Application Gateway
description: This article provides information on how to enable cookie-based affinity with an Application Gateway. 
services: application-gateway
author: greg-lindsay
ms.service: azure-application-gateway
ms.topic: how-to
ms.date: 9/17/2024
ms.author: greglin
---

# Enable Cookie based affinity with an Application Gateway
As outlined in the [Azure Application Gateway Documentation](./application-gateway-components.md#http-settings), Application Gateway supports cookie based affinity, which means it can direct subsequent traffic from a user session to the same server for processing.

> [!TIP]
> Also see [What is Application Gateway for Containers](for-containers/overview.md).

## Example
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: guestbook
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/cookie-based-affinity: "true"
spec:
  rules:
  - http:
      paths:
      - backend:
          serviceName: frontend
          servicePort: 80
```