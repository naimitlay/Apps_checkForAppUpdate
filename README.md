# Apps_checkForAppUpdate


// ১. ইন-অ্যাপ আপডেট ইনিশিয়ালাইজেশন (Modern ActivityResultLauncher)
        // =========================================================================
        appUpdateManager = AppUpdateManagerFactory.create(this);

        updateActivityResultLauncher = registerForActivityResult(
                new ActivityResultContracts.StartIntentSenderForResult(),
                result -> {
                    if (result.getResultCode() != RESULT_OK) {
                        Toast.makeText(MainActivity.this, "অ্যাপটি চালুর জন্য আপডেট করা বাধ্যতামূলক!", Toast.LENGTH_SHORT).show();
                        finish(); // অ্যাপ বন্ধ হয়ে যাবে
                    }
                }
        );

        checkForAppUpdate();


// =========================================================================





private void checkForAppUpdate() {
        Task<AppUpdateInfo> appUpdateInfoTask = appUpdateManager.getAppUpdateInfo();

        appUpdateInfoTask.addOnSuccessListener(appUpdateInfo -> {
            if (appUpdateInfo.updateAvailability() == UpdateAvailability.UPDATE_AVAILABLE
                    && appUpdateInfo.isUpdateTypeAllowed(AppUpdateType.IMMEDIATE)) {

                try {
                    appUpdateManager.startUpdateFlowForResult(
                            appUpdateInfo,
                            updateActivityResultLauncher,
                            AppUpdateOptions.newBuilder(AppUpdateType.IMMEDIATE).build());
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        });
    }//end checkForAppUpdate
