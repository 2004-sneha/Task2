package com.springboot.Controller;

import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;


import org.json.JSONObject;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class WeatherApp {
	private static final String API_KEY = "b4ce00e2d00c467a9ca165243250402";
    private static final String API_URL = "http://api.openweathermap.org/data/2.5/weather?q=%s&appid=%s";
    private static final List<String> CITIES = Arrays.asList("Pune", "Mumbai", "Delhi");

    @GetMapping("/weather")
    public List<String> getWeather() {
        HttpClient client = HttpClient.newHttpClient();

        return CITIES.stream().map(city -> {
            String url = String.format(API_URL, city, API_KEY);
            HttpRequest request = HttpRequest.newBuilder()
                    .uri(URI.create(url))
                    .build();

            try {
                HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
                JSONObject jsonObject = new JSONObject(response.body());

                String weather = jsonObject.getJSONArray("weather").getJSONObject(0).getString("description");
                double temperature = jsonObject.getJSONObject("main").getDouble("temp") - 273.15; // Convert from Kelvin to Celsius
                int humidity = jsonObject.getJSONObject("main").getInt("humidity");

                return String.format("Weather in %s:\nDescription: %s\nTemperature: %.2f°C\nHumidity: %d%%",
                        city, weather, temperature, humidity);
            } catch (Exception e) {
                e.printStackTrace();
                return "Unable to fetch weather data for " + city;
            }
        }).collect(Collectors.toList());
    }

}
