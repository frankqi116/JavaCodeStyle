  This is a basic example about using LocalBroadcastManager

  The example code shows case:
    1. async way of get firbase token.
    2. then notify UI that registration has completed, so the progress indicator can be hidden.


  Code in MainActivity

    // Register for GCM
    private void setupGCM(final SharedPreferences prefs) {
        Timber.d("-\nStarting RegistrationIntentService");
        FirebaseInstanceId.getInstance().getInstanceId().addOnSuccessListener(this, new OnSuccessListener<InstanceIdResult>() {
            @Override
            public void onSuccess(InstanceIdResult instanceIdResult) {
                // after child thread success call back
                String token = instanceIdResult.getToken();
                IWS.setToken(token);
                prefs.edit().putString(IWS.KEY__GCM_DEVICE_TOKEN, token).apply();
                prefs.edit().putBoolean(IWS.GCM__STORED_TOKEN, true).apply();

                // Notify UI that registration has completed, so the progress indicator can be hidden.
                Intent registrationComplete = new Intent(IWS.GCM__REGISTRATION_COMPLETE);
                registrationComplete.putExtra(IWS.KEY__GCM_STORED_TOKEN, true);
                LocalBroadcastManager.getInstance(MainActivity.this).sendBroadcast(registrationComplete);
            }
        });
    }

  Code in SplashFragment

    // Register brodcast receiver
    private BroadcastReceiver mRegistrationBroadcastReceiver = new BroadcastReceiver() {

        @Override
        public void onReceive(Context context, Intent intent) {
            mHasGCMToken = intent.getBooleanExtra(IWS.KEY__GCM_STORED_TOKEN, false);
            // update UI
            startupFlow();
        }
    };