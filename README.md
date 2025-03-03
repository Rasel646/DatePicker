build.gradel   implementation 'joda-time:joda-time:2.9.4' 



activity_main.xml

<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">


    <Button
        android:id="@+id/btndatepic"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:layout_centerInParent="true"
        android:textColor="@color/black"
        style="?attr/spinnerStyle"
        android:onClick="btnShow"



        />

    <Button
        android:id="@+id/btnShow"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/btndatepic"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="10dp"
        android:text="Calculate"


        />

    <TextView
        android:id="@+id/tvDisplay"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/btnShow"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="10dp"



        />

    <TextView
        android:id="@+id/tvDisplay2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/tvDisplay"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="10dp"



        />

</RelativeLayout>



MainActivity.java


package com.companyname.datepicker;

import android.app.AlertDialog;
import android.app.DatePickerDialog;
import android.icu.text.SimpleDateFormat;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.DatePicker;
import android.widget.TextView;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

import org.joda.time.Period;
import org.joda.time.PeriodType;

import java.util.Calendar;
import java.util.Date;

public class MainActivity extends AppCompatActivity {

    DatePickerDialog datePickerDialog;
    Button btnDatePicker,btnShow;
    TextView tvDisplay,tvDisplay2;
    String snnDate;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        setContentView(R.layout.activity_main);

        btnDatePicker=findViewById(R.id.btndatepic);
        tvDisplay=findViewById(R.id.tvDisplay);
        btnShow=findViewById(R.id.btnShow);
        tvDisplay2=findViewById(R.id.tvDisplay2);




        indatepicker();


        btnShow.setOnClickListener(v -> {

             Calendar cal = Calendar.getInstance();
             int year = cal.get(Calendar.YEAR);
             int month = cal.get(Calendar.MONTH);
             int day = cal.get(Calendar.DAY_OF_MONTH);

             String sdate = snnDate;
             String edate = day + "/" + (month+1) + "/" + year;

            SimpleDateFormat sdf = new SimpleDateFormat("dd/MM/yyyy");

            try {

                Date date1=sdf.parse(sdate);
                Date date2=sdf.parse(edate);


                Period period=new Period(date1.getTime(),date2.getTime(), PeriodType.yearMonthDay());

                int year1=period.getYears();
                int month1=period.getMonths();
                int day1=period.getDays();

                tvDisplay.setText("Years: "+year1+" \nMonths: "+month1+" \nDays: "+day1);

//             ==================================================================================
                long diff=date2.getTime()-date1.getTime();
                long days=diff/(24*60*60*1000);
                long months=days/30;
                long years=months/12;
                long weeks=days/7;


                tvDisplay2.setText("Days: "+days+" \nMonths: "+months+" \nYears: "+years+" \nWeeks: "+weeks );





            } catch (Exception e) {
                e.printStackTrace();
            }




        });













    }

    private void indatepicker() {

        DatePickerDialog.OnDateSetListener dateSetListener = new DatePickerDialog.OnDateSetListener() {
            @Override
            public void onDateSet(DatePicker view, int year, int month, int day) {



                String date = getstringformate(year, month, day);
                btnDatePicker.setText(date);

                String snDate= day + "/" + (month+1) + "/" + year;
                snnDate=snDate;

            }};

            Calendar calendar = Calendar.getInstance();

            int years = calendar.get(Calendar.YEAR);

            int months = calendar.get(Calendar.MONTH);

            int days = calendar.get(Calendar.DAY_OF_MONTH);

        String date = days + "/" + makedates(months) + "/" + years;
        btnDatePicker.setText(date);



        int style = AlertDialog.THEME_HOLO_DARK;

            datePickerDialog =new

            DatePickerDialog(MainActivity .this, style, dateSetListener, years, months, days);
            datePickerDialog.getDatePicker().setMaxDate(System.currentTimeMillis());






    }

    private String getstringformate(int year, int month, int day) {

        return day + "/" + makedates(month)+ "/" + year;
    }


    private String makedates(int months) {
        months = months + 1;

        if(months==1)
            return "JAN";
        if(months==2)
            return "FEB";
        if(months==3)
            return "MAR";
        if(months==4)
            return "APR";
        if (months==5)
            return "MAY";
        if(months==6)
            return "JUN";
        if(months==7)
            return "JUL";
        if(months==8)
            return "AUG";
        if(months==9)
            return "SEP";
        if(months==10)
            return "OCT";
        if(months==11)
            return "NOV";
        if(months==12)
            return "DEC";



        return "JAN";
    }


    public void btnShow(View view) {
        datePickerDialog.show();



    }
}
