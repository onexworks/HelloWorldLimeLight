# HelloWorldLimeLight
OnexWorks! Here you go! ReadMe
curl -X 'POST' -H 'Content-Type:application/json' -d @dft-index-task.json http://<IPAddress>:8090/druid/indexer/v1/task
<html>
<head>
<meta http-equiv="Content-Type" content="text/html;charset=ISO-8859-1"/>
<title>Error 401 </title>
</head>
<body>
<h2>HTTP ERROR: 401</h2>
<p>Problem accessing /druid/indexer/v1/task. Reason:
<pre>    Authentication required</pre></p>
<hr /><a href="http://eclipse.org/jetty">Powered by Jetty:// 9.3.19.v20170502</a><hr/>
</body>
</html>


//cat dft-index-task.json
{
  "type" : "index_hadoop",
  "spec" : {
    "ioConfig" : {
      "type" : "hadoop",
      "inputSpec" : {
        "type" : "static",
        "paths" : "/home/rpolish/dftData/dftraw.json"
      }
    },
    "dataSchema" : {
      "dataSource" : "dftsample",
      "granularitySpec" : {
        "type" : "uniform",
        "segmentGranularity" : "day",
        "queryGranularity" : "none",
        "intervals" : ["07-05-2011/06-29-2012"]
      },
      "parser" : {
        "type" : "hadoopyString",
        "parseSpec" : {
          "format" : "json",
          "dimensionsSpec" : {
            "dimensions" : [
              "volume",
              "market_flag",
              "sales_condition",
              "exclude_record_flag"
            ]
          },
          "timestampSpec" : {
            "format" : "auto",
            "column" : "tr_time"
          }
        }
      },
      "metricsSpec" : [
        {
          "name" : "count",
          "type" : "count"
        },
        {
          "name" : "averageprice",
          "type" : "longSum",
          "fieldName" : "price"
        },
        {
          "name" : "priceunfiltered",
          "type" : "longSum",
          "fieldName" : "unfiltered_price"
        }
      ]
    },
    "tuningConfig" : {
      "type" : "hadoop",
      "partitionsSpec" : {
        "type" : "hashed",
        "targetPartitionSize" : 5000000
      },
      "jobProperties" : {}
    }
  }
}

