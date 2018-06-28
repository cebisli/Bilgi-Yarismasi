# Bilgi-Yarismasi

public class Basla extends AppCompatActivity implements View.OnClickListener {
    SQLiteDatabase db;
    TextView txtZaman, txtSoru;
    Button btnA, btnB, btnC, btnD, btnSonraki;
    Context context = this;
    //CountDownTimer countDownTimer;
    final int[] sayilar = new int[10];
    String cevap = "";
    int sayac = 0;
    MediaPlayer fonMuzik;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.basla);
        init();
     //   zamaniBaslat();
        sayilariOlustur();
        soruGetir(sayac);
        btnSonraki.setEnabled(false);
        fonMuzik=MediaPlayer.create(context,R.raw.fon);
        fonMuzik.start();
    }

    @Override
    protected void onStop() {
        super.onStop();
        db.close();
    }

    public void soruGetir(int sayi) {
        DatabaseHelper dbHelper = new DatabaseHelper(this);
        try {
            dbHelper.CreateDataBase();
            db = dbHelper.getReadableDatabase();
            String[] getColumnName = {"soru,cevap,A,B,C,D"};
            Cursor crs = db.query("sorular", getColumnName, "_id = ?", new String[]{String.valueOf(sayilar[sayi])}, null, null, null);

            if (crs.getCount() > 0) {
                crs.moveToFirst();
                txtSoru.setText(crs.getString(crs.getColumnIndex("soru")));
                btnA.setText(crs.getString(crs.getColumnIndex("A")));
                btnB.setText(crs.getString(crs.getColumnIndex("B")));
                btnC.setText(crs.getString(crs.getColumnIndex("C")));
                btnD.setText(crs.getString(crs.getColumnIndex("D")));
                cevap = crs.getString(crs.getColumnIndex("cevap"));
            }
            crs.close();
            db.close();

        } catch (Exception ex) {
            Toast.makeText(context, "Toplam puanınız 100", Toast.LENGTH_LONG).show();
            Log.w("hata", "Veritabanı oluşturulamadı ve kopyalanamadı!");
        }
    }

    public void sayilariOlustur() {
        int i = 0, j = 0, uret = 0;
        Random r = new Random();
        boolean varmi = false;

        while (i < 10) {
            uret = r.nextInt(120);
            varmi = false;
            for (j = 0; j < 10; j++) {
                if (sayilar[j] == uret) {
                    varmi = true;
                    break;
                }
            }

            if (varmi != true) {
                sayilar[i] = uret;
                i++;
            }
        }

    }

    public void init() {
       // txtZaman = (TextView) findViewById(R.id.txtZaman);
        txtSoru = (TextView) findViewById(R.id.txtSoru);
        btnA = (Button) findViewById(R.id.btnA);
        btnB = (Button) findViewById(R.id.btnB);
        btnC = (Button) findViewById(R.id.btnC);
        btnD = (Button) findViewById(R.id.btnD);
        btnSonraki = (Button) findViewById(R.id.btnSonraki);

        btnA.setOnClickListener(this);
        btnB.setOnClickListener(this);
        btnC.setOnClickListener(this);
        btnD.setOnClickListener(this);
        btnSonraki.setOnClickListener(this);
    }

    @Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {

        if(keyCode==KeyEvent.KEYCODE_BACK)
        {
            AlertDialog.Builder alert = new AlertDialog.Builder(context);
            alert.setTitle("Çıkmak İstediğinizden Eminmisiniz ?");
            alert.setPositiveButton("Evet", new DialogInterface.OnClickListener() {
                public void onClick(DialogInterface arg0, int arg1) {
                    //evet seçilmesi durumunda yapılacak işlemler
                    // finish ile activity'i sonlandırıyoruz.
                    finish();
                    fonMuzik.stop();
                }
            });
            alert.setNegativeButton("Hayır", new DialogInterface.OnClickListener() {
                public void onClick(DialogInterface arg0, int arg1) {
                    //hayır seçildiginde yapılacak işlemler
                }
            });

            alert.show();

        }

        return super.onKeyDown(keyCode, event);
    } // Geri tuşuna bastığımızda olması gerekenler

    /* private void zamaniBaslat() {
        countDownTimer = new CountDownTimer(10000, 600) {
            @Override
            public void onTick(long millisUntilFinished) {
                txtZaman.setText(String.valueOf(millisUntilFinished / 500));
            }

            @Override
            public void onFinish() {
                txtZaman.setText("FINISH");
            }
        }.start();
    }*/

   private void yanlisCevapMesajı(){

       android.support.v7.app.AlertDialog.Builder mesaj = new android.support.v7.app.AlertDialog.Builder(context);
       mesaj.setMessage("Maalesef yanlış cevap!\n Doğru cevap=" + cevap+"\n Toplam Puanınız = "+(sayac*10));
       mesaj.setCancelable(false);
       mesaj.setPositiveButton("Ana Menü", new DialogInterface.OnClickListener() {
           @Override
           public void onClick(DialogInterface dialog, int which) {
               finish();
               fonMuzik.stop();
           }
       }).show();


   }
    @Override
    public void onClick(View v) {
        switch (v.getId()) {

            case R.id.btnA:
                if ( btnA.getText().equals(cevap)) {
                    Toast.makeText(context, "Tebrikler Doğru Cevap!!!\n Toplam puanınız : "+(sayac+1)*10, Toast.LENGTH_SHORT).show();
                    btnSonraki.setEnabled(true);
                } else {
                    yanlisCevapMesajı();
                }
                btnA.setEnabled(false);
                btnB.setEnabled(false);
                btnC.setEnabled(false);
                btnD.setEnabled(false);

                break;

            case R.id.btnB:
                if (btnB.getText().equals(cevap)) {
                    Toast.makeText(context, "Tebrikler Doğru Cevap!!!\n Toplam puanınız : "+(sayac+1)*10, Toast.LENGTH_SHORT).show();
                    btnSonraki.setEnabled(true);
                } else {
                    yanlisCevapMesajı();
                }
                btnA.setEnabled(false);
                btnB.setEnabled(false);
                btnC.setEnabled(false);
                btnD.setEnabled(false);

                break;

            case R.id.btnC:
                if (btnC.getText().equals(cevap)) {
                    Toast.makeText(context, "Tebrikler Doğru Cevap!!!\n Toplam puanınız : "+(sayac+1)*10, Toast.LENGTH_SHORT).show();
                    btnSonraki.setEnabled(true);
                } else {
                    yanlisCevapMesajı();
                }
                btnA.setEnabled(false);
                btnB.setEnabled(false);
                btnC.setEnabled(false);
                btnD.setEnabled(false);

                break;

            case R.id.btnD:
                if (btnD.getText().equals(cevap)) {
                    Toast.makeText(context, "Tebrikler Doğru Cevap!!!\n Toplam puanınız : "+(sayac+1)*10, Toast.LENGTH_SHORT).show();
                    btnSonraki.setEnabled(true);
                } else {
                    yanlisCevapMesajı();
                }
                btnA.setEnabled(false);
                btnB.setEnabled(false);
                btnC.setEnabled(false);
                btnD.setEnabled(false);

                break;

            case R.id.btnSonraki:
                sayac++;
                soruGetir(sayac);
               // zamaniBaslat();
                btnA.setEnabled(true);
                btnB.setEnabled(true);
                btnC.setEnabled(true);
                btnD.setEnabled(true);
                btnSonraki.setEnabled(false);

                if (sayac > 9) {
                    android.support.v7.app.AlertDialog.Builder mesaj = new android.support.v7.app.AlertDialog.Builder(context);
                    mesaj.setTitle("Yarışma Sonucu");
                    mesaj.setMessage("Toplam puanınız");
                    mesaj.setCancelable(false);
                    mesaj.setMessage("Toplam Puanınız = "+(sayac*10));
                    mesaj.setPositiveButton("Ana Menü", new DialogInterface.OnClickListener() {
                        @Override
                        public void onClick(DialogInterface dialog, int which) {
                            finish();
                        }
                    }).show();

                    fonMuzik.stop();
                    btnSonraki.setEnabled(false);
                }
                break;
        }
    }
}

