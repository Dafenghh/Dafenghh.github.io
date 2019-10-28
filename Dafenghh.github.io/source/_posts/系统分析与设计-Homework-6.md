---
title: 系统分析与设计 Homework 6
comments: true
categories: SystemDesign
tags:
  - homework
abbrlink: 78defc7e
date: 2019-07-04 20:24:50
---

# 使用 UMLet 建模

## 使用类图，分别对 Asg_RH 文档中 Make Reservation 用例以及 Payment 用例开展领域建模。然后，根据上述模型，给出建议的数据表以及主要字段，特别是主键和外键

> 注意事项：
> 对象必须是名词、特别是技术名词、报表、描述类的处理；
> 关联必须有多重性、部分有名称与导航方向
> 属性要注意计算字段
> 数据建模，为了简化描述仅需要给出表清单，例如：
> Hotel（ID/Key，Name，LoctionID/Fkey，Address…..）

![Make Reservation](/img/system_design_homework/homework6/hotel_domain.png)

```
Hotel(hotelID/key, address)
RoomCatalog(catalogID/key, hotelID/Fkey)
RoomDescription(descriptionID/key, catalogID/Fkey, price, type, address)
Room(roomID/key, date, descriptionID/Fkey, availability)
ReservationLineItem(roomID/Fkey, reservationID/Fkey, adultCount, childCount)
Reservation(revervationID/key, customerID/Fkey)
Customer(name, customerID/key)
```

![Payment](/img/system_design_homework/homework6/payment_domain.png)

```
Customer(name, customerID/key)
Card(cardID/key, customerID/Fkey, bank, number)
Payment(paymentID/key, customerID/Fkey, cardID/Fkey, totalCost)
Item(itemID/key, paymentID/Fkey, details)

```

## 使用 UML State Model，对每个订单对象生命周期建模
> 建模对象： 参考 Asg_RH 文档， 对 Reservation/Order 对象建模。
> 建模要求： 参考练习不能提供足够信息帮助你对订单对象建模，请参考现在 定旅馆 的旅游网站，尽可能分析围绕订单发生的各种情况，直到订单通过销售事件（柜台销售）结束订单。

![State Model](/img/system_design_homework/homework6/hotel_states.png)