``` java
    File file = new File("D:\\安全漏洞\\xxe-atack.xml");
    //        File file = new File("D:\\安全漏洞\\normal.xml");
        String xmlStr;

        DocumentBuilderFactory dbFactory = DocumentBuilderFactory.newInstance();

        // 禁用外部实体
        dbFactory.setExpandEntityReferences(false);
        dbFactory.setFeature("http://apache.org/xml/features/disallow-doctype-decl",true);
        dbFactory.setFeature("http://xml.org/sax/features/external-general-entities",false);
        dbFactory.setFeature("http://xml.org/sax/features/external-parameter-entities",false);

        DocumentBuilder dBuilder = dbFactory.newDocumentBuilder();
        Document doc = dBuilder.parse(file);
        DOMSource domSource = new DOMSource(doc);
        StringWriter writer = new StringWriter();
        StreamResult result = new StreamResult(writer);
        TransformerFactory tf = TransformerFactory.newInstance();
        Transformer transformer = tf.newTransformer();
        transformer.transform(domSource, result);
        xmlStr = writer.toString();

        System.out.println(xmlStr);
```
