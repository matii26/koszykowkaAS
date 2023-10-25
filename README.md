# koszykowkaAS
Main
package com.egzamin.koszykowka;

import androidx.appcompat.app.AppCompatActivity;
import androidx.lifecycle.Observer;
import androidx.lifecycle.ViewModelProvider;

import android.os.Bundle;
import android.view.View;
import android.widget.TextView;

import com.egzamin.koszykowka.databinding.ActivityMainBinding;

public class MainActivity extends AppCompatActivity {

    private ActivityMainBinding binding; //nazwa od pliku xml
    private Integer punkty = 0;
    private PustyViewModel punktyViewModel;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        binding = ActivityMainBinding.inflate(getLayoutInflater());
        View view = binding.getRoot();
        setContentView(view);
        punktyViewModel = new ViewModelProvider(this)
                .get(PustyViewModel.class);
        punktyViewModel.getPunkty().observe(this, new Observer<Integer>() {
            @Override
            public void onChanged(Integer integer) {
                binding.textView.setText(integer.toString());
            }
        });
        binding.button.setOnClickListener(
                new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        punktyViewModel.dodajPunkty(1);

                    }
                }
        );
        binding.button2.setOnClickListener(
                new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        punktyViewModel.dodajPunkty(2);

                    }
                }
        );
        binding.button3.setOnClickListener(
                new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        punktyViewModel.dodajPunkty(3);

                    }
                }
        );

    }
}

PunktyActivity

package com.egzamin.koszykowka;

import androidx.lifecycle.MutableLiveData;
import androidx.lifecycle.ViewModel;

public class PustyViewModel extends ViewModel {
    private MutableLiveData<Integer> punkty;

    public MutableLiveData<Integer> getPunkty() {
        if (punkty == null)
            punkty = new MutableLiveData<>();
            punkty.setValue(0);
        return punkty;
    }

    public void setPunkty(MutableLiveData<Integer> punkty) {
        this.punkty = punkty;
    }

    public void dodajPunkty(int ile){
        if(punkty.getValue() == null){
            punkty.setValue(0);
        }
        else{
            punkty.setValue(punkty.getValue()+ile);
        }
    }
}

Manifest
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher_kosz"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_kosz_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Koszykowka"
        tools:targetApi="34">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>

            <meta-data
                android:name="android.app.lib_name"
                android:value="" />
        </activity>
    </application>

</manifest>

build.gradle

plugins {
    id 'com.android.application'
}

android {
    namespace 'com.egzamin.koszykowka'
    compileSdk 34

    defaultConfig {
        applicationId "com.egzamin.koszykowka"
        minSdk 26
        targetSdk 34
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    buildFeatures{
        viewBinding true
        dataBinding true
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
implementation 'androidx.lifecycle:lifecycle-viewmodel:2.6.2'
    implementation 'androidx.lifecycle:lifecycle-livedata:2.6.2'
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
    testImplementation 'junit:junit:4.13.2'
    androidTestImplementation 'androidx.test.ext:junit:1.1.5'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.5.1'
}

layout\activity_main

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/Gray"
    tools:context=".MainActivity">

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.2"
        app:srcCompat="@drawable/ic_baseline_sports_basketball_24" />

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="0"
        android:textSize="99sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toTopOf="@id/button"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/button"
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:textSize="32sp"
        android:backgroundTint="@color/black"
        app:layout_constraintEnd_toStartOf="@id/button2"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/textView"
        app:layout_constraintBottom_toBottomOf="parent"
        android:text="+1"
        />

    <Button
        android:id="@+id/button2"
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:textSize="32sp"
        android:backgroundTint="@color/black"
        app:layout_constraintStart_toEndOf="@id/button"
        app:layout_constraintEnd_toStartOf="@id/button3"
        app:layout_constraintTop_toTopOf="@id/button"
        android:text="+2" />

    <Button
        android:id="@+id/button3"
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:textSize="32sp"
        android:backgroundTint="@color/black"
        app:layout_constraintTop_toTopOf="@id/button"
        app:layout_constraintStart_toEndOf="@id/button2"
        app:layout_constraintEnd_toEndOf="parent"
        android:text="+3" />

</androidx.constraintlayout.widget.ConstraintLayout>
