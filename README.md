# Jamef-Calculo-frete-Java
**Como começar**

Criando a sua conta no Jamef

https://www.jamef.com.br/jamef/ecp/comunidade.do?app=portal

1.Coloque o CNPJ da sua empresa

2.Coloque a UF do seu estado (SP, RJ, MG, etc)

3.Coloque o Número da Filial da Jamef

4.Coloque o nome do usuario do login
#
```java
public class JamefHttp {

    private final String USER_AGENT = "Mozilla/5.0";

    public static void main(String[] args) throws Exception {

        int TIPTRA = 1;
        String CNPJCPF = "20147617002276";
        String MUNORI = "COTIA";
        String ESTORI = "SP";
        String SEGPROD = "000004";
        double PESO = 15.0;
        double VALMER = 10.0;
        double METRO3 = 1 * 0.40 * 0.40 * 0.40;
        String CEPDES = "06719680";
        String FILCOT = "07";
        int DIA = Calendar.getInstance().get(Calendar.DAY_OF_MONTH);
        int MES = Calendar.getInstance().get(Calendar.MONDAY) + 1;
        int ANO = Calendar.getInstance().get(Calendar.YEAR);
        String USUARIO = "jameff";

        double valor = 0;
        String prazoEntrega = "";

        JamefHttp http = new JamefHttp();
        String response = http.sendGet(TIPTRA, CNPJCPF, MUNORI, ESTORI, SEGPROD, PESO, VALMER, METRO3, CEPDES, FILCOT, DIA, MES, ANO, USUARIO);

        try {
            JSONObject jsonobject = new JSONObject(response.toString());
            valor = jsonobject.getDouble("valor");
            prazoEntrega = jsonobject.getString("previsao_entrega");

        } catch (JSONException err) {
            err.getMessage();
        }
         NumberFormat calculo;
         calculo = NumberFormat.getCurrencyInstance(new Locale("pt", "BR"));
         calculo.setMinimumFractionDigits(2);
         DecimalFormat df = new DecimalFormat("0");
        
        System.out.println(calculo.format(valor));
        System.out.println(prazoEntrega);
        System.out.println("Dias "+SomaDiaData(prazoEntrega));
       

    }

    // HTTP GET request
    private String sendGet(int TIPTRA, String CNPJCPF, String MUNORI, String ESTORI, String SEGPROD, double PESO, double VALMER, double METRO3, String CEPDES, String FILCOT, int DIA, int MES, int ANO, String USUARIO) throws Exception {

        String url = " https://www.jamef.com.br/frete/rest/v1/" + TIPTRA + "/" + CNPJCPF + "/" + MUNORI + "/" + ESTORI + "/" + SEGPROD + "/" + PESO + "/" + VALMER + "/" + METRO3 + "/" + CEPDES + "/" + FILCOT + "/" + DIA + "/" + MES + "/" + ANO + "/" + USUARIO;

        URL obj = new URL(url);
        HttpURLConnection con = (HttpURLConnection) obj.openConnection();

        // optional default is GET
        con.setRequestMethod("GET");

        //add request header
        con.setRequestProperty("User-Agent", USER_AGENT);
        String urlParameters = "";

        int responseCode = con.getResponseCode();
        System.out.println("\nSending 'GET' request to URL : " + url);
        System.out.println("Response Code : " + responseCode);

        BufferedReader in = new BufferedReader(
                new InputStreamReader(con.getInputStream()));
        String inputLine;
        StringBuffer response = new StringBuffer();

        while ((inputLine = in.readLine()) != null) {
            response.append(inputLine);
        }
        in.close();

        //print result
        System.out.println(response.toString());
        return response.toString();

    }

    public static Long SomaDiaData(String dataPrazo) {
        SimpleDateFormat sdf = new SimpleDateFormat("dd/MM/yyyy");
        Date dataBase = new Date();
        Date vencimento = null;
        String dateBaseformatada = sdf.format(dataBase);
        try {
            dataBase = sdf.parse(dateBaseformatada);
            vencimento = sdf.parse(dataPrazo);
        } catch (java.text.ParseException e) {
            //   return;
        }

        Long diferencaMS = vencimento.getTime() - dataBase.getTime();
        Long diferencaSegundos = diferencaMS / 1000;
        Long diferencaMinutos = diferencaSegundos / 60;
        Long diferencaHoras = diferencaMinutos / 60;
        Long diferencaDias = diferencaHoras / 24;

        return diferencaDias;
    }

}
```
