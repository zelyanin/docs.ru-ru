---
title: Объект My.Request
ms.date: 07/20/2015
f1_keywords:
- My.MyWebExtension.Request
- My.Request
helpviewer_keywords:
- My.Request object
ms.assetid: 93d5f0e2-6b60-4a2c-8652-d90216f6ad10
ms.openlocfilehash: 22329bc501c9bb75b1336dd5384ab5b23a98ac21
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350681"
---
# <a name="myrequest-object"></a>Объект My.Request
Возвращает объект <xref:System.Web.HttpRequest> для запрашиваемой страницы.  
  
## <a name="remarks"></a>Заметки  
 Объект `My.Request` содержит сведения о текущем HTTP-запросе.  
  
 Объект `My.Request` доступен только для приложений ASP.NET.  
  
## <a name="example"></a>Пример  
 The following example gets the header collection from the `My.Request` object and uses the `My.Response` object to write it to the ASP.NET page.  
  
 [!code-aspx-vb[VbVbalrMyWeb#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyWeb/VB/Default.aspx#1)]  
  
## <a name="see-also"></a>См. также

- <xref:System.Web.HttpRequest>
- [Объект My.Response](../../../visual-basic/language-reference/objects/my-response-object.md)
