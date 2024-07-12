# challenge-oracle-decoder

import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.io.IOException;

public class ConsultaDivisa {

    public double consultarTasaCambio(String baseCurrency, String targetCurrency) throws IOException {
        URI uri = URI.create("https://v6.exchangerate-api.com/v6/9cd110368347b56af2328d1c/latest/USD" + baseCurrency);

        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(uri)
                .build();

        try {
            HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
            if (response.statusCode() == 200) {
                // Parsear la respuesta JSON para obtener la tasa de cambio
                String responseBody = response.body();
                double exchangeRate = parseExchangeRate(responseBody, targetCurrency);
                return exchangeRate;
            } else {
                throw new IOException("Error al consultar la API. Código de estado: " + response.statusCode());
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            throw new IOException("Interrupción durante la consulta de la tasa de cambio.");
        }
    }

    private double parseExchangeRate(String json, String targetCurrency) {

        return 0.0;
    }
}




import java.io.IOException;
import java.util.Scanner;
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
public class Principal {
    public static void main(String[] args) throws IOException {
        Scanner lectura = new Scanner(System.in);
        ConsultaDivisa consulta = new ConsultaDivisa();

        System.out.println("Ingrese la divisa base (ej. USD, EUR, GBP): ");
        String baseCurrency = lectura.nextLine().toUpperCase();

        System.out.println("Ingrese la divisa objetivo (ej. EUR, GBP, JPY): ");
        String targetCurrency = lectura.nextLine().toUpperCase();

        try {
            double exchangeRate = consulta.consultarTasaCambio(baseCurrency, targetCurrency);
            System.out.printf("Tasa de cambio de %s a %s es: %.4f%n", baseCurrency, targetCurrency, exchangeRate);
        } catch (IOException e) {
            System.out.println("Error al consultar la tasa de cambio: " + e.getMessage());
        }
    }
}


