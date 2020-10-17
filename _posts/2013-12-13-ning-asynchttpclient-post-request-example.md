---
layout: post
title: Ning Asynchttpclient Post Request Example
categories: program java
---

Well, it has been a while that i did not update anything on my blog. It's time.

On my task, an async post request need to be sent. So i found ning's asyncHttpClient. It's so great and also support websocket. That could improve my project even better.

Today's chanllenge: send a async post request via ning's AsyncHttpClient

I can't find any example about the post request. All examples on their demo are about get request. That block me for around 1 hour.

{% highlight java %}
import com.ning.http.client.AsyncCompletionHandler;
import com.ning.http.client.AsyncHttpClient;
import com.ning.http.client.Request;
import com.ning.http.client.RequestBuilder;
import com.ning.http.client.Response;

public class TaskFlowManager {
    String url;
    public void start() {
        final AsyncHttpClient asyncHttpClient = new AsyncHttpClient();
        Request req = new RequestBuilder("POST")
                .setUrl(url)
                .addHeader("Content-Type", "application/x-www-form-urlencoded")
                .addParameter("ID", "12")
                .build();
        try {
            asyncHttpClient.prepareRequest(req).execute(
                    new AsyncCompletionHandler<Response>() {

                        @Override
                        public Response onCompleted(Response response)
                                throws Exception {
                            // Do something with the Response
                            // ...
                            System.out.println("Finished");
                            System.out.println(response
                                    .getResponseBody("utf-8"));
                            asyncHttpClient.close();
                            return response;
                        }

                        @Override
                        public void onThrowable(Throwable t) {
                            // Something wrong happened.
                        }
                    });
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        while (!asyncHttpClient.isClosed()) {
            System.out.println("Wait and sleep");
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
    }
}
{% endhighlight %}
At beginning, i have no idea where to put the parameters on the request. Some article said that parameters could in body of post request as the follows:
{% highlight java %}
String data = "ID=12&Name=12";
asyncHttpClient.setBody(data);
{% endhighlight %}
That is not working. Because the server need the request content type should be form-data or x-www-form-urlencoded. Finally, i put the content type and keypair data in head of request. That works:
{% highlight java %}
Request req = new RequestBuilder("POST")
                .setUrl(url)
                .addHeader("Content-Type", "application/x-www-form-urlencoded")
                .addParameter("ID", "12")
                .build();
asyncHttpClient.prepareRequest(req);
{% endhighlight %}
Here is the respose in json:
{% highlight json %}
Wait and sleep
Finished
{"status":"OK","ID":"12"}
{% endhighlight %}