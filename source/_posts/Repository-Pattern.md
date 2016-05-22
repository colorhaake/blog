---
title: Repository Pattern
date: 2016-05-22 11:38:48
tags: Repository Pattern
---
# Repository Pattern

一般在設計較為複雜的大型應用程式時，會建議將程式依各部分不同元件的屬件去劃分成不同區塊，方便去撰寫、控管不同區塊的程式。  
Microsoft建議可以將程式，粗略分成三層式的[架構](https://msdn.microsoft.com/en-us/library/ff648105.aspx)，如下圖

{% asset_img 'three-layered-services-application.png' %}

簡單來說：Presentation Layer是用來置放有關於UI相關的程式；Business Layer是處理Presenstaion Layer和Data Layer之前相關的邏輯；Data Layer負責資料相關的儲存、查詢

由上圖可以得知，在Business Layer和Data Layer是具有相依關係的，也就是說一般我們在撰寫程式時，常常會將我們的資料存在資料庫、硬碟或者雲端之中；但是假如將來有一天，程式使用的資料庫系統突然不敷需求，可能要從MySQL換到Oracle，問題是Business Layer和Data Layer的關係是綁定的，原本在Business Layer使用的MySQL API在Oracle上面不存在或者介面長的不一樣，所以開發者要花費大量的工夫去一一檢查、置換程式，會造成有非常大的風險，可能置換完的結果會出錯，勞財勞神。

[Repository Pattern](https://msdn.microsoft.com/en-us/library/ff649690)就是要來解決這樣的問題。他的架構如下圖表示

{% asset_img 'repository-pattern.png' %}

就是在Business Layer和Data Layer之間多了一層Repository Pattern層，Business Layer完全不知道他用的Data Layer是用什麼樣的系統。Business Layer要的很簡單，就是他發出的query可以拿到正確的資料，也因此他發出來的query的介面是一致的，只是實際上實做出來的結果是跟不同系統有相關的，MySQL就是用MySQL的介面實作，Oracle就用Oracle。

{% asset_img 'repository-pattern-detail.png' %}

如此一來，假如真的有一天我們要換Data Layer，query的介面是一樣的，我只需要換不同的實作就好了。

這麼做的好處就是，程式劃分的非常的乾淨，Business Layer只相依於Repository Pattern；Data Layer綁定於Repository Pattern，在測試維護上面，因為只變動實作的部分，我們只需要關心不同實作出來的資料是不是正確的就好，而不需要花費大量的工夫來測試全部的區塊。
