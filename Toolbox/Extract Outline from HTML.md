# Extract Outline from HTML

Tuesday, September 20, 2016
4:49 PM

```XML
<?xml version="1.0" encoding="utf-8"?><xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"    xmlns:msxsl="urn:schemas-microsoft-com:xslt" exclude-result-prefixes="msxsl">    <xsl:output method="xml" indent="yes"/>     <xsl:template match="@* | node()">        <xsl:copy>            <xsl:apply-templates select="@* | node()"/>        </xsl:copy>    </xsl:template>   <xsl:template match="blockquote" />  <xsl:template match="div" />  <xsl:template match="ol" />  <xsl:template match="p" />  <xsl:template match="table" />  <xsl:template match="ul" /></xsl:stylesheet>
```
