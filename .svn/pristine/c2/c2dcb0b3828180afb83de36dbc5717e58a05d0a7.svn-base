package com.macaron.FashionKing;

import android.annotation.SuppressLint;
import android.app.Activity;
import android.content.ActivityNotFoundException;
import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import android.view.KeyEvent;
import android.view.LayoutInflater;
import android.view.View;
import android.webkit.WebChromeClient;
import android.webkit.WebView;
import android.webkit.WebViewClient;
import android.widget.FrameLayout;

@SuppressLint("Override")
public class MainActivity extends Activity {
	private WebView webView;
	private FrameLayout customViewContainer;
	private WebChromeClient.CustomViewCallback customViewCallback;
	private View mCustomView;
	private myWebChromeClient mWebChromeClient;
	private myWebViewClient mWebViewClient;

	/**
	 * Called when the activity is first created.
	 */
	@SuppressLint("SetJavaScriptEnabled")
	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);
		customViewContainer = (FrameLayout) findViewById(R.id.customViewContainer);
		webView = (WebView) findViewById(R.id.webView);

		mWebViewClient = new myWebViewClient();
		webView.setWebViewClient(mWebViewClient);

		mWebChromeClient = new myWebChromeClient();
		webView.setWebChromeClient(mWebChromeClient);
		webView.getSettings().setJavaScriptEnabled(true);
		webView.getSettings().setAppCacheEnabled(true);
		webView.getSettings().setBuiltInZoomControls(true);
		webView.getSettings().setSaveFormData(true);
		webView.loadUrl("http://www.bnsfolio.com/fk");
	}

	public boolean inCustomView() {
		return (mCustomView != null);
	}

	public void hideCustomView() {
		mWebChromeClient.onHideCustomView();
	}

	@Override
	protected void onPause() {
		super.onPause(); // To change body of overridden methods use File |
							// Settings | File Templates.
		webView.onPause();
	}

	@Override
	protected void onResume() {
		super.onResume(); // To change body of overridden methods use File |
							// Settings | File Templates.
		webView.onResume();
	}

	@Override
	protected void onStop() {
		super.onStop(); // To change body of overridden methods use File |
						// Settings | File Templates.
		if (inCustomView()) {
			hideCustomView();
		}
	}

	@Override
	public boolean onKeyDown(int keyCode, KeyEvent event) {
		if (keyCode == KeyEvent.KEYCODE_BACK) {

			if (inCustomView()) {
				hideCustomView();
				return true;
			}

			if ((mCustomView == null) && webView.canGoBack()) {
				webView.goBack();
				return true;
			}
		}
		return super.onKeyDown(keyCode, event);
	}

	class myWebChromeClient extends WebChromeClient {
		private View mVideoProgressView;

		public void onShowCustomView(View view, int requestedOrientation,
				CustomViewCallback callback) {
			onShowCustomView(view, callback); // To change body of overridden
												// methods use File | Settings |
												// File Templates.
		}

		@Override
		public void onShowCustomView(View view, CustomViewCallback callback) {

			// if a view already exists then immediately terminate the new one
			if (mCustomView != null) {
				callback.onCustomViewHidden();
				return;
			}
			mCustomView = view;
			webView.setVisibility(View.GONE);
			customViewContainer.setVisibility(View.VISIBLE);
			customViewContainer.addView(view);
			customViewCallback = callback;
		}

		@Override
		public View getVideoLoadingProgressView() {

			if (mVideoProgressView == null) {
				LayoutInflater inflater = LayoutInflater
						.from(MainActivity.this);
				mVideoProgressView = inflater.inflate(R.layout.video_progress,
						null);
			}
			return mVideoProgressView;
		}

		@Override
		public void onHideCustomView() {
			super.onHideCustomView(); // To change body of overridden methods
										// use File | Settings | File Templates.
			if (mCustomView == null)
				return;

			webView.setVisibility(View.VISIBLE);
			customViewContainer.setVisibility(View.GONE);

			// Hide the custom view.
			mCustomView.setVisibility(View.GONE);

			// Remove the custom view from its container.
			customViewContainer.removeView(mCustomView);
			customViewCallback.onCustomViewHidden();
			webView.reload();
			mCustomView = null;
		}
	}

	private class myWebViewClient extends WebViewClient {

		public static final String INTENT_PROTOCOL_START = "intent:";
		public static final String INTENT_PROTOCOL_INTENT = "#Intent;";
		public static final String INTENT_PROTOCOL_END = ";end;";
		public static final String GOOGLE_PLAY_STORE_PREFIX = "market://details?id=";

		@Override
		public boolean shouldOverrideUrlLoading(WebView view, String url) {
			/*
			 * Log.d("URL", url); String v =
			 * String.valueOf(android.os.Build.VERSION.SDK_INT);
			 * Log.d("VERSION", v);
			 * android.os.Build.VERSION.SDK_INT >= 19 안드로이드 4.4 이상인 경우
			 */
			if (android.os.Build.VERSION.SDK_INT >= 19) {
				if (url.startsWith(INTENT_PROTOCOL_START)) {
					
					final int customUrlStartIndex = INTENT_PROTOCOL_START.length();
					final int customUrlEndIndex = url.indexOf(INTENT_PROTOCOL_INTENT);
					if (customUrlEndIndex < 0) {
						return false;
					} else {
						final String customUrl = url.substring(customUrlStartIndex, customUrlEndIndex);
						Intent intent = new Intent(Intent.ACTION_VIEW);
						intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
						try {
							intent.setData(Uri.parse(customUrl));
							getBaseContext().startActivity(intent);
						} catch (ActivityNotFoundException e) {
							final int packageStartIndex = customUrlEndIndex+ INTENT_PROTOCOL_INTENT.length();
							final int packageEndIndex = url.indexOf(INTENT_PROTOCOL_END);

							final String packageName = url.substring(packageStartIndex,	packageEndIndex < 0 ? url.length()	: packageEndIndex);
							intent.setData(Uri.parse(GOOGLE_PLAY_STORE_PREFIX	+ packageName));
							getBaseContext().startActivity( intent );
						}
						return true;
					}
				} else {
					return false;
				}
			} else {
				if (url.startsWith("intent:") || url.startsWith("kakaolink:") || url.startsWith("market:")) {
					Intent intent = new Intent(Intent.ACTION_VIEW,Uri.parse(url));
					startActivity(intent);
				} else {
					view.loadUrl(url);
				}
				return super.shouldOverrideUrlLoading(view, url);
			}
			// if (url.startsWith(INTENT_PROTOCOL_START)) {
		}
		// return super.shouldOverrideUrlLoading(view, url); //To change body of
		// overridden methods use File | Settings | File Templates.

	}
}