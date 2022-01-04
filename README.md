# coursera-test
Coursera test repository  ONBACKPRESSED()

# SD CARD:
package com.example.final9writeandreaddataintosdcard;

import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import java.io.BufferedReader;
import java.io.File;
import java.io.FileOutputStream;
import java.io.FileReader;
import java.nio.charset.StandardCharsets;
public class MainActivity extends AppCompatActivity {
    EditText fname,fcont;
    Button read,write;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        fname = (EditText) findViewById(R.id.file_name);
        fcont = (EditText) findViewById(R.id.file_cont);
        read = (Button) findViewById(R.id.btn_read);
        write = (Button) findViewById(R.id.btn_write);
        read.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                try{
                    String inp="",text="";
                    File file = new File(getFilesDir(),fname.getText().toString());
                    BufferedReader bf = new BufferedReader(new FileReader(file));
                    while((inp=bf.readLine())!=null) text+=inp;
                    fcont.setText(text);
                }catch(Exception e){
                    e.printStackTrace();
                }
            }
        });
        write.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                try{
                    String inp="",text="";
                    File file = new File(getFilesDir(),fname.getText().toString());
                    FileOutputStream fp = new FileOutputStream(file);
                    fp.write(fcont.getText().toString().getBytes());
                    fp.close();
                    fcont.setText("");
                }catch(Exception e){
                    e.printStackTrace();
                }
            }
        });
    }
}


Activity_main.xml:
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
    <EditText
        android:id="@+id/file_name"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:layout_marginEnd="79dp"
        android:ems="10"
        android:inputType="textPersonName"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
    <EditText
        android:id="@+id/file_cont"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="25dp"
        android:ems="10"
        android:gravity="start|top"
        android:inputType="textMultiLine"
        app:layout_constraintStart_toStartOf="@+id/file_name"
        app:layout_constraintTop_toBottomOf="@+id/file_name" />
    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:layout_marginTop="25dp"
        android:text="File Name"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
    <TextView
        android:id="@+id/textView2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="18dp"
        android:text="Contents"
        app:layout_constraintBottom_toBottomOf="@+id/file_cont"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/file_cont" />
    <Button
        android:id="@+id/btn_read"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="67dp"
        android:layout_marginBottom="35dp"
        android:text="read"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent" />
    <Button
        android:id="@+id/btn_write"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginEnd="82dp"
        android:layout_marginBottom="36dp"
        android:text="Write"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>



# ALERT
mainactivity.java
package com.app.alert;

import android.app.NotificationChannel;
import android.app.NotificationManager;
import android.app.PendingIntent;
import android.content.Intent;
import android.os.Build;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.NotificationCompat;
import androidx.core.app.NotificationManagerCompat;
public class MainActivity extends AppCompatActivity{
    Button notify;
    EditText e;
    private void createNotificationChannel() {
        String channel_name = "test_channel";
        String channel_description = "test_desc";
        String CHANNEL_ID = "1";
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            CharSequence name = channel_name;
            String description = channel_description;
            int importance = NotificationManager.IMPORTANCE_DEFAULT;
            NotificationChannel channel = new NotificationChannel(CHANNEL_ID, name, importance);
            channel.setDescription(description);
            NotificationManager notificationManager = getSystemService(NotificationManager.class);
            notificationManager.createNotificationChannel(channel);
        }    }
    @Override
    public void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        notify= (Button) findViewById(R.id.button);
        e= (EditText) findViewById(R.id.editText);
        createNotificationChannel();
        notify.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v){
                String CHANNEL_ID = "1";
                Intent intent = new Intent(MainActivity.this, SecondActivity.class);
                intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_CLEAR_TASK);
                PendingIntent pendingIntent = PendingIntent.getActivity(MainActivity.this, 0, intent, 0);
                NotificationCompat.Builder builder = new NotificationCompat.Builder(MainActivity.this, CHANNEL_ID)
                        .setSmallIcon(R.drawable.ic_launcher_background)
                        .setContentTitle("My notification")
                        .setContentText(e.getText().toString())
                        .setPriority(NotificationCompat.PRIORITY_DEFAULT)
                        .setContentIntent(pendingIntent)
                        .setAutoCancel(true);
                NotificationManagerCompat notificationManager = NotificationManagerCompat.from(MainActivity.this);
                int notificationId = 1;
                notificationManager.notify(notificationId, builder.build());
            }   });   }  }
            
            # secondactivity.java:
            package com.app.alert;

import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;
public class SecondActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);
    }
}


