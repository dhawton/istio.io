---
title: Чи може Istio відправляти інформацію про трейсинг на зовнішній сервер сумісний з Zipkin?
weight: 70
---

Так, для цього потрібно використовувати повне доменне імʼя сумісного з Zipkin екземпляру. Наприклад: `zipkin.mynamespace.svc.cluster.local`.