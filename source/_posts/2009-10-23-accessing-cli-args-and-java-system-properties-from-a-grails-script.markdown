---
author: mjwall
comments: false
date: 2009-10-23 19:25:38+00:00
layout: post
slug: accessing-cli-args-and-java-system-properties-from-a-grails-script
title: Accessing CLI args and java system properties from a Grails script
wordpress_id: 109
alias: /2009/10/accessing-cli-args-and-java-system-properties-from-a-grails-script/
categories:
- grails
---

Quick post so I can remember how to access CLI args and Java system properties inside of a grails script.

Put this following code inside of your_grails_app/scripts/ScriptTest.groovy


    target ( default : 'Print args and java system properties' ) {
        //grails script-test arg1 arg2
        println "Grails CLI args"
        println Ant.project.properties."grails.cli.args"

        //grails -Dprop1anything script-test
        println "Java system properties of prop1"
        println Ant.project.properties.prop1
    }


So now you can run the following
`
$grails -Dprop1=anything script-test arg1 arg2
Welcome to Grails 1.1.1 - http://grails.org/
Licensed under Apache Standard License 2.0
Grails home is set to: /Library/Grails/Home

Base Directory: /Users/mjwall/src/sample1
Running script /Users/mjwall/src/sample1/ScriptTest.groovy
Grails CLI args
arg1
arg2
Java system properties of prop1
anything
`
