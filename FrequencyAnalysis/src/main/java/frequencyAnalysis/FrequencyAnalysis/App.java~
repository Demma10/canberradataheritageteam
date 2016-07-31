package frequencyAnalysis.FrequencyAnalysis;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Writer;
import java.util.ArrayList;
import java.util.List;

import org.apache.http.HttpResponse;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.json.JSONArray;
import org.json.JSONObject;
import org.jsoup.Jsoup;
import org.jsoup.examples.HtmlToPlainText;

/**
 * Prototype program designed to retrieve newspaper articles from Trove. This
 * program dumps the files into a text file, in a real application this program
 * would either dump the text into a database or pipe it to another program.
 * The only text cleanup this program does is strip the HTML tags outside of
 * the article. A real production-quality application would strip out or
 * correct the OCR artefacts.
 *
 */
public class App 
{
    	public static void main(String[] args) throws ClientProtocolException, IOException {
    			  CloseableHttpClient client = HttpClients.createDefault();
    			  HttpGet request = new HttpGet("http://api.trove.nla.gov.au/result?q=attitude&zone=newspaper&key=iochseh3vhsakek9&encoding=json&n=60&l-cateogry=Article&l-title=10&l-year=1939");
    			  HttpResponse response = client.execute(request);
    			  BufferedReader rd = new BufferedReader (new InputStreamReader(response.getEntity().getContent()));
    			  
    			  StringBuilder sb = new StringBuilder();

    			    String line;
    			    while ((line = rd.readLine()) != null) {
    			        sb.append(line);
    			    }

    			   //System.out.println(sb);
    			    
    			    JSONObject json = new JSONObject(sb.toString());
    			  
    			  JSONArray resp = json.getJSONObject("response").getJSONArray("zone").getJSONObject(0).getJSONObject("records").getJSONArray("article");
    			  

    			  JSONObject jsonChildObject;
    			  List<String> ids = new ArrayList<String>();
    			  for(int i = 0; i < resp.length(); i++) {

             //DO what ever you whont with jsonChildObject 

    				  jsonChildObject = resp.getJSONObject(i);
            ids.add((String) jsonChildObject.get("id"));
           }
    			  
    			  //Loop over each id and grab the article text.
    			  String restPrefix = "http://api.trove.nla.gov.au/newspaper/";
    			  String parameters = "?key=iochseh3vhsakek9&encoding=json&include=articletext";
    			  
    			  List<String> articleBodies = new ArrayList<String>();
    			  List<String> urls = new ArrayList<String>();
    			  for(String id : ids) {
        			  HttpGet request2 = new HttpGet(restPrefix + id + parameters);
        			  HttpResponse response2 = client.execute(request2);
        			  BufferedReader rd2 = new BufferedReader (new InputStreamReader(response2.getEntity().getContent()));
        			  
        			  StringBuilder sb2 = new StringBuilder();

        			    String line2;
        			    while ((line2 = rd2.readLine()) != null) {
        			        sb2.append(line2);
        			    }
        			    
        			    //System.out.println("Article: " + id);
        			    //System.out.println(sb2);
        			    
        			    JSONObject json2 = new JSONObject(sb2.toString());
        			    String plain = new HtmlToPlainText().getPlainText(Jsoup.parse(json2.getJSONObject("article").getString("articleText")));
        			    articleBodies.add(plain);
        			    urls.add(json2.getJSONObject("article").getString("troveUrl"));
        			    
        			    //System.out.println(json2.getJSONObject("article").getString("articleText"));
    			  }
    			  
    			  System.out.println("Article ids: ");
    			  System.out.println(ids);
    			  System.out.println();
    			  System.out.println("Article bodies: ");
    			  System.out.println(articleBodies);
    			  System.out.println();
    			  System.out.println("Urls: ");
    			  System.out.println(urls);
    			  
    			  client.close();
    			  
    			  //Write the output to file. With over 20 articles it's too big for the console.
    			  
    			  System.out.println("Writing output to file.");
    			  try (Writer writer = new BufferedWriter(new OutputStreamWriter(
    		              new FileOutputStream("onehundred_articles_clean.txt"), "utf-8"))) {
    		   writer.write("");
    		   writer.write("Article ids: ");
    		   writer.write(ids.toString());
    		   writer.write("\n");
    		   writer.write("Article bodies: ");
    		   writer.write(articleBodies.toString());
    		   writer.write("\n");
    		   writer.write("Urls: ");
    		   writer.write(urls.toString());
    		}

    			  System.out.println("Finished writing to file onehundred_articles_clean.txt");

    	}
}
