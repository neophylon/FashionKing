package com.macaron.FashionKing;

 
import com.macaron.FashionKing.R;
import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.os.Handler;
  
public class SplashActivity extends Activity {
	
	// Splash screen timer
	private static int SPLASH_TIME_OUT = 1000;


	@Override
	protected void onCreate(Bundle savedInstanceState) {
		
		super.onCreate(savedInstanceState);
		setContentView(R.layout.splash);

		new Handler().postDelayed(new Runnable() {

			@Override
			public void run() {

				// Getting module,idx from intent
				Intent i = getIntent();		

				// This method will be executed once the timer is over Start your app main activity
				Intent ii = new Intent(SplashActivity.this, MainActivity.class);
	        
				startActivity(ii);
				finish();
			}
		}, SPLASH_TIME_OUT);
	}

}