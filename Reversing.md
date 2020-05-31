# 1337 Skills -  hxp 36C3 CTF 


Aqui temos um exemplo de uma chall de engenharia reversa que contém um arquivo .apk 

     App: https://play.google.com/store/apps/details?id=com.progressio.wildskills
     Connection: nc 88.198.154.132 7002
     
Nesse challenge, o apk necessitava de um código de ativação para se reproduzido. 

Então, descompilando esse .apk, chegamos ao seguinte código: 

##### MainActivity

    public void activateApp(View view) {
        int i;
       try {
           i = Integer.parseInt(this.editTextActivation.getText().toString());
        } catch (NumberFormatException unused) {
            i = -1;
       }
       Calendar instance = Calendar.getInstance();
        if (i == ((int) (Math.pow((double) (instance.get(3) * instance.get(1)), 2.0d) % 999983.0d))) {
           findViewById(R.id.scrollViewActivation).setVisibility(4);
            ((InputMethodManager) getSystemService("input_method")).hideSoftInputFromWindow(this.editTextActivation.getWindowToken(), 0);
           SharedPreferences.Editor edit = this.prefsmain.edit();
           edit.putBoolean("Activated", true);
           long time = new Date().getTime();
            edit.putLong("Installed", time);
           edit.putLong("ActivationDate", time);
            edit.commit();
           return;
       }
        Toast.makeText(this, "Ungültiger Aktivierungscode", 1).show();
        this.editTextActivation.requestFocus();
       ((InputMethodManager) getSystemService("input_method")).showSoftInput(this.editTextActivation, 1);
    }
    
 
 Sendo assim, temos que o nosso código de ativação está presente na seguinte função: 
 
   ##### ((int) (Math.pow((double) (instance.get(3) * instance.get(1)), 2.0d) % 999983.0d))
