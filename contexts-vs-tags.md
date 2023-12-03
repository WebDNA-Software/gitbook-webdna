---
description: >-
  WebDNA uses Contexts and Tags to communicate the programming logic to the
  WebDNA engine where it is rendered and results returned to the visitor's
  browser via the web server in the form of HTML
---

# Contexts vs Tags

### Contexts and Tags

Throughout the documentation that is outlined on this website, each context or tag is noted as such in the headline.

The difference between a contexts and a tags is the way that it is written, simply a context has an opening and a closing, where a tag is a command enclosed in square brackets ie \[ & ].

**Format of a WebDNA context:**

```
[context] ..... [/context]Note that the opening of the context is enclosed in square brackets and the closing of the context is also enclosed with square brackets with the addition of a forward slash / to indicate that it is the closure of the context.
```

**To list all the contexts that WebDNA uses:**

```
[listallcontexts]The result is an unformatted list:
! addfields addlineitem append appendfile arrayget arrayset boldwords calccrc32 capitalize case convertchars convertwords countchars countwords decrypt default else encrypt exclusivelock fileinfo format formvariables founditems function getchars grep hide hideif html1 html2 html3 if input interpret jsonstore jsonstore2 lineitems listchars listcookies listdatabases listfields listfiles listmimeheaders listpath listvariables listwords loop lowercase math mbgetchars mblistchars middle orderfile raw regex removehtml replace replacefounditems return returnraw scope search sendmail setheader setlineitem shell showif shownext spawn sql sqlconnect sqlexecute sqlinfo sqlresult store switch table tcpconnect tcpsend text then unurl uppercase url waitforfile writefile wsdlboundmessageparts wsdlencstyles wsdlmessagebindingprotocoldata wsdlmessagespec wsdlmimeformatalternatives wsdlmimemrparts wsdlmsgbindingspecs wsdlmsgpartspec wsdlmsgpartspecs wsdlopbindingspecs wsdlopprotocoldata wsdlparse wsdlportbindingprotocoldata wsdlportbindingspec wsdlportspecs wsdlsoapbody wsdlsoapheaderfaults wsdlsoapheaders wsdlsrvcspecs xmlcreatenode xmlnode xmlnodeattributes xmlnodes xmlparse xsdanyspec xsdattrspecs xsdcontentspec xsdcontentspecnodes xsdeltspec xsdstfacets xsdtypespec xsl xslt
```

**Format of a WebDNA tag:**

```
[tag]Note that a tag does not have an opening and closing, it is simply the tag enclosed in square brackets, in some cases a list of parameters may be added
```

**To list all the tags that WebDNA uses:**

```
[listalltags]The result is an unformatted list:
xmldata isdemo version isdeveloper date time random lastrandom lastautonumber last_wdna_error lastordernumber deletetable delete removelineitem clearlineitems purchase validcard ofsencrypt ofsdecrypt maxcopies maxdomains expiredate listallcontexts listalltags include protect flushdatabases flushcache closedatabase commitdatabase authenticate lookup wdna_isp_setting elapsedtime freememory platform applicationtype product sandboxid numdbsallowed prefsdb deletefile deletefolder createfolder copyfile movefile renamefile movefolder renamefolder copyfolder calcfilecrc32 filecompare findstring wsdldocumentation sqlrelease sqldisconnect deletefields webserver recall sessionip sessiondate sessiontime sessionutime sessionlife browseridmatch sessionipmatch sessionalive sessionstart sessionend wait biotype dbencrypt createdb documentroot ipaddress realip referrer referer browsername issecureclient username password command thisurl thisurlplusget thisfile thishost islicensed isaccelerated isbtlicensed iseclicensed redirect permredirect getcookie setcookie setmimeheader getmimeheader rawpost cart
```

It is the square brackets that makes WebDNA stand apart from other languages such as PHP and distinct from the angular brackets used by html.
