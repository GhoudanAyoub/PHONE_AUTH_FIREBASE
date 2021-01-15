# PHONE_AUTH_FIREBASE
JUST COPYY PAST THE CODE AND HERE YOU GO :)

# build.gradle app

```sh

    implementation 'com.google.firebase:firebase-core:18.0.0'
    implementation 'com.google.firebase:firebase-auth:20.0.1'
    implementation 'com.firebaseui:firebase-ui-database:4.3.1'
```

# JAVA

```sh

public class phoneTest extends Fragment {

        private static final int MY_PERMISSIONS_REQUEST_SEND_SMS = 1;
        private EditText editText, N1, N2, N3, N4, N5, N6;
        private Button sendCodeButton;
        private Button phoneDone;
        private Button ResendCode;
        private TextView ChangePhoneNumber;
        private LinearLayout FirstLayout, SecondLayout;
        private String phone;
        private CountryCodePicker countryCodePicker;
        private View root;
        private String Code;
        private PhoneAuthProvider.ForceResendingToken mResendToken;

        PhoneAuthProvider.OnVerificationStateChangedCallbacks mCallbacks = new PhoneAuthProvider.OnVerificationStateChangedCallbacks() {
            @Override
            public void onVerificationCompleted(@NotNull PhoneAuthCredential phoneAuthCredential) {
                enableSecondLayout();
                EnablePhoneDoneButton();
                Code = phoneAuthCredential.getSmsCode();
            }

            @Override
            public void onVerificationFailed(@NotNull FirebaseException e) {
                if (e instanceof FirebaseAuthInvalidCredentialsException) {
                    Toast.makeText(getActivity(), getString(R.string.ProblemConnectionWithYourPhone), Toast.LENGTH_SHORT).show();
                } else if (e instanceof FirebaseTooManyRequestsException) {
                    Toast.makeText(getActivity(), getString(R.string.TooManyRequest), Toast.LENGTH_SHORT).show();
                }
            }

            @Override
            public void onCodeSent(@NotNull String s, @NotNull PhoneAuthProvider.ForceResendingToken forceResendingToken) {
                super.onCodeSent(s, forceResendingToken);
                mResendToken = forceResendingToken;
                Log.e("TAG", "onCodeSent: "+s );
            }
        };

        @Override
        public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
            root = inflater.inflate(R.layout.phone_test_fragment, container, false);
            view(root);
            checkForSmsPermission();
            enableFirstLayout();

            ChangePhoneNumber.setOnClickListener(v3 -> {
                enableFirstLayout();
                sendCodeButton.setVisibility(View.VISIBLE);
                phoneDone.setVisibility(View.GONE);
            });
            sendCodeButton.setOnClickListener(v -> sendVerificationCode());
            ResendCode.setOnClickListener(v -> resendVerificationCode(phone, mResendToken));
            phoneDone.setOnClickListener(v4 -> verifySignInCode());

            return root;
        }

        //**** UI Manipulation
        public void view(View root) {
            N1 = root.findViewById(R.id.N1);
            N2 = root.findViewById(R.id.N2);
            N3 = root.findViewById(R.id.N3);
            N4 = root.findViewById(R.id.N4);
            N5 = root.findViewById(R.id.N5);
            N6 = root.findViewById(R.id.N6);
            ChangePhoneNumber = root.findViewById(R.id.ChangePhoneNumber);
            sendCodeButton = root.findViewById(R.id.sendCodeButton);
            SecondLayout = root.findViewById(R.id.Secondlayout);
            Button makeCall = root.findViewById(R.id.makeCall);
            FirstLayout = root.findViewById(R.id.firstLayout);
            ResendCode = root.findViewById(R.id.resendCode);
            countryCodePicker = root.findViewById(R.id.ccp);
            phoneDone = root.findViewById(R.id.phonedone);
            editText = root.findViewById(R.id.phoneText);
            makeCall.setEnabled(false);
        }

        private boolean validateForm() {
            boolean valid = true;

            String email1 = editText.getText().toString();
            if (TextUtils.isEmpty(email1)) {
                editText.setError(getString(R.string.FillEveryPlace));
                valid = false;
            } else {
                editText.setError(null);
            }
            if (email1.length() != 9) {
                editText.setError(getString(R.string.putYourPhoneNumber));
                valid = false;
            } else {
                editText.setError(null);
            }
            return valid;
        }

        private boolean validateForm2() {
            boolean valid = true;

            String email1 = N1.getText().toString();
            if (TextUtils.isEmpty(email1)) {
                N1.setError(getString(R.string.FillEveryPlace));
                valid = false;
            } else {
                N1.setError(null);
            }
            String email2 = N2.getText().toString();
            if (TextUtils.isEmpty(email2)) {
                N2.setError(getString(R.string.FillEveryPlace));
                valid = false;
            } else {
                N2.setError(null);
            }
            String email3 = N3.getText().toString();
            if (TextUtils.isEmpty(email3)) {
                N3.setError(getString(R.string.FillEveryPlace));
                valid = false;
            } else {
                N3.setError(null);
            }
            String email4 = N4.getText().toString();
            if (TextUtils.isEmpty(email4)) {
                N4.setError(getString(R.string.FillEveryPlace));
                valid = false;
            } else {
                N4.setError(null);
            }
            String email5 = N5.getText().toString();
            if (TextUtils.isEmpty(email5)) {
                N5.setError(getString(R.string.FillEveryPlace));
                valid = false;
            } else {
                N5.setError(null);
            }
            String email6 = N6.getText().toString();
            if (TextUtils.isEmpty(email6)) {
                N6.setError(getString(R.string.FillEveryPlace));
                valid = false;
            } else {
                N6.setError(null);
            }
            return valid;
        }

        private void disableSmsButton() {
            Toast.makeText(getContext(), R.string.sms_disabled, Toast.LENGTH_LONG).show();
            sendCodeButton.setVisibility(View.GONE);
            phoneDone.setVisibility(View.GONE);
        }

        private void enableSecondLayout() {
            FirstLayout.setVisibility(View.GONE);
            SecondLayout.setVisibility(View.VISIBLE);
        }

        private void enableFirstLayout() {
            FirstLayout.setVisibility(View.VISIBLE);
            SecondLayout.setVisibility(View.GONE);
        }

        private void EnablePhoneDoneButton() {
            sendCodeButton.setVisibility(View.GONE);
            phoneDone.setVisibility(View.VISIBLE);
        }

        //******core
        private void sendVerificationCode() {
            if (!validateForm()) return;
            phone = "+" + countryCodePicker.getSelectedCountryCode() + editText.getText().toString();
            PhoneAuthProvider.verifyPhoneNumber(
                    PhoneAuthOptions.newBuilder(FireBaseClient.GetInstance().getFirebaseAuth())
                            .setPhoneNumber(phone)
                            .setTimeout(30L, TimeUnit.SECONDS)
                            .setActivity(requireActivity())
                            .setCallbacks(mCallbacks)
                            .build());
            enableSecondLayout();
            EnablePhoneDoneButton();
        }

        private void resendVerificationCode(String phoneNumber, PhoneAuthProvider.ForceResendingToken token) {
            PhoneAuthProvider.verifyPhoneNumber(
                    PhoneAuthOptions.newBuilder(FireBaseClient.GetInstance().getFirebaseAuth())
                            .setPhoneNumber(phoneNumber)
                            .setTimeout(60L, TimeUnit.SECONDS)
                            .setActivity(requireActivity())
                            .setCallbacks(mCallbacks)
                            .setForceResendingToken(token)
                            .build());
        }

        private void verifySignInCode() {
            if (!validateForm2()) return;
            String txt = "" + N1.getText().toString() + "" + N2.getText().toString() + "" + N3.getText().toString() + "" + N4.getText().toString() + "" + N5.getText().toString() + "" + N6.getText().toString();
            if (txt.equals(Code)) {
                Toast.makeText(getActivity(), getString(R.string.VerificationPhoneSucced), Toast.LENGTH_SHORT).show();
                Bundle bundle = new Bundle();
                bundle.putString("phone", phone);
                Navigation.findNavController(root).navigate(R.id.gmailTest, bundle);
            } else {
                // TODO: 2020-12-09 Delete THis block later att the end
                Bundle bundle = new Bundle();
                bundle.putString("phone", phone);
                Navigation.findNavController(root).navigate(R.id.gmailTest, bundle);
            }
        }

        //**** Permission
        @Override
        public void onRequestPermissionsResult(int requestCode, @NotNull String[] permissions, @NotNull int[] grantResults) {
            if (requestCode == MY_PERMISSIONS_REQUEST_SEND_SMS) {
                if (permissions[0].equalsIgnoreCase(Manifest.permission.SEND_SMS)
                        && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                    sendCodeButton.setVisibility(View.VISIBLE);
                } else {
                    Toast.makeText(getContext(), getString(R.string.failure_permission), Toast.LENGTH_LONG).show();
                    disableSmsButton();
                }
            }
        }

        private void checkForSmsPermission() {
            if (ActivityCompat.checkSelfPermission(requireActivity(), Manifest.permission.SEND_SMS) != PackageManager.PERMISSION_GRANTED) {
                ActivityCompat.requestPermissions(requireActivity(), new String[]{Manifest.permission.SEND_SMS}, MY_PERMISSIONS_REQUEST_SEND_SMS);
                sendCodeButton.setVisibility(View.VISIBLE);
            } else {
                phoneDone.setVisibility(View.GONE);
            }
        }
}

```


# XML SIDE 

```sh

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".ui.Account.phoneTest">

    <ImageView
        android:id="@+id/imageView2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginTop="40dp"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@drawable/logo" />

    <LinearLayout
        android:id="@+id/firstLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="32dp"
        android:orientation="vertical"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/imageView2">

        <TextView
            android:id="@+id/textView4"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginStart="8dp"
            android:layout_marginLeft="8dp"
            android:layout_marginEnd="16dp"
            android:layout_marginRight="16dp"
            android:fontFamily="@font/catamaran"
            android:gravity="start"
            android:text="Personal Phone Number"
            android:textColor="@android:color/black"
            android:textSize="18sp"
            android:textStyle="bold" />

        <TextView
            android:id="@+id/textView11"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginStart="8dp"
            android:layout_marginLeft="8dp"
            android:layout_marginEnd="16dp"
            android:layout_marginRight="16dp"
            android:fontFamily="@font/catamaran"
            android:gravity="start"
            android:text="Make sure that your Phone Number is correcte"
            android:textColor="@android:color/darker_gray"
            android:textSize="16sp" />

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginStart="16dp"
            android:layout_marginLeft="16dp"
            android:layout_marginTop="32dp"
            android:layout_marginEnd="16dp"
            android:layout_marginRight="16dp"
            android:gravity="center_vertical">

            <com.hbb20.CountryCodePicker
                android:id="@+id/ccp"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                app:ccp_defaultNameCode="MA" />

            <EditText
                android:id="@+id/phoneText"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_alignParentBottom="true"
                android:hint="634_457855"
                android:inputType="phone"
                android:singleLine="true" />
        </LinearLayout>
    </LinearLayout>

    <LinearLayout
        android:id="@+id/Secondlayout"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:orientation="vertical"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/firstLayout">

        <TextView
            android:id="@+id/textView12"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginStart="8dp"
            android:layout_marginLeft="8dp"
            android:layout_marginTop="33dp"
            android:layout_marginEnd="16dp"
            android:layout_marginRight="16dp"
            android:fontFamily="@font/catamaran"
            android:gravity="start"
            android:text="Check your Phone Number "
            android:textColor="@android:color/black"
            android:textSize="18sp"
            android:textStyle="bold"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintHorizontal_bias="1.0"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent" />

        <TextView
            android:id="@+id/textView13"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginStart="8dp"
            android:layout_marginLeft="8dp"
            android:layout_marginEnd="16dp"
            android:layout_marginRight="16dp"
            android:fontFamily="@font/catamaran"
            android:gravity="start"
            android:text="Entre The Code"
            android:textColor="@android:color/darker_gray"
            android:textSize="14sp"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintHorizontal_bias="1.0"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/textView12" />

        <TextView
            android:id="@+id/ChangePhoneNumber"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginStart="8dp"
            android:layout_marginLeft="8dp"
            android:layout_marginEnd="16dp"
            android:layout_marginRight="16dp"
            android:fontFamily="@font/catamaran"
            android:gravity="start"
            android:text="Change your Phone Number"
            android:textColor="@android:color/holo_red_dark"
            android:textSize="14sp"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintHorizontal_bias="1.0"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/textView13" />

        <TableRow
            android:id="@+id/tableRow"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginStart="16dp"
            android:layout_marginLeft="16dp"
            android:layout_marginTop="76dp"
            android:layout_marginEnd="16dp"
            android:layout_marginRight="16dp"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintHorizontal_bias="0.666"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/ChangePhoneNumber">

            <EditText
                android:id="@+id/N1"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="2dp"
                android:layout_marginRight="2dp"
                android:layout_weight="1 "
                android:ems="10"
                android:fontFamily="@font/black_ops_one"
                android:gravity="center"
                android:inputType="number"
                android:maxLength="1"
                android:singleLine="true"
                android:textColor="@color/colorPrimary"
                android:textSize="18sp" />

            <EditText
                android:id="@+id/N2"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="2dp"
                android:layout_marginRight="2dp"
                android:layout_weight="1 "
                android:ems="10"
                android:fontFamily="@font/black_ops_one"
                android:gravity="center"
                android:inputType="number"
                android:maxLength="1"
                android:singleLine="true"
                android:textColor="@color/colorPrimary"
                android:textSize="18sp" />

            <EditText
                android:id="@+id/N3"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="2dp"
                android:layout_marginRight="2dp"
                android:layout_weight="1 "
                android:ems="10"
                android:fontFamily="@font/black_ops_one"
                android:gravity="center"
                android:inputType="number"
                android:maxLength="1"
                android:singleLine="true"
                android:textColor="@color/colorPrimary"
                android:textSize="18sp" />

            <EditText
                android:id="@+id/N4"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="2dp"
                android:layout_marginRight="2dp"
                android:layout_weight="1 "
                android:ems="10"
                android:fontFamily="@font/black_ops_one"
                android:gravity="center"
                android:inputType="number"
                android:maxLength="1"
                android:singleLine="true"
                android:textColor="@color/colorPrimary"
                android:textSize="18sp" />

            <EditText
                android:id="@+id/N5"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="2dp"
                android:layout_marginRight="2dp"
                android:layout_weight="1"
                android:ems="10"
                android:fontFamily="@font/black_ops_one"
                android:gravity="center"
                android:inputType="number"
                android:maxLength="1"
                android:singleLine="true"
                android:textColor="@color/colorPrimary" />

            <EditText
                android:id="@+id/N6"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="2dp"
                android:layout_marginRight="2dp"
                android:layout_weight="1"
                android:ems="10"
                android:fontFamily="@font/black_ops_one"
                android:gravity="center"
                android:inputType="number"
                android:maxLength="1"
                android:singleLine="true"
                android:textColor="@color/colorPrimary" />
        </TableRow>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_margin="5dp"
            android:gravity="center"
            android:orientation="horizontal">

            <Button
                android:id="@+id/resendCode"
                android:layout_width="159dp"
                android:layout_height="51dp"
                android:layout_margin="8dp"
                android:layout_marginBottom="8dp"
                android:background="@drawable/cirlce"
                android:fontFamily="@font/catamaran_bold"
                android:text="RESEND"
                android:textColor="@color/colorPrimary"
                app:layout_constraintBottom_toTopOf="@+id/NumberNext"
                app:layout_constraintEnd_toStartOf="@+id/textView15"
                app:layout_constraintStart_toStartOf="parent"
                app:layout_constraintTop_toBottomOf="@+id/tableRow"
                app:layout_constraintVertical_bias="0.142" />

            <TextView
                android:id="@+id/textView15"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginBottom="8dp"
                android:fontFamily="@font/catamaran_bold"
                android:text="OR"
                android:textColor="@android:color/black"
                android:textSize="14sp"
                app:layout_constraintBottom_toTopOf="@+id/NumberNext"
                app:layout_constraintEnd_toEndOf="parent"
                app:layout_constraintStart_toStartOf="parent"
                app:layout_constraintTop_toBottomOf="@+id/tableRow"
                app:layout_constraintVertical_bias="0.168" />

            <Button
                android:id="@+id/makeCall"
                android:layout_width="163dp"
                android:layout_height="54dp"
                android:layout_margin="8dp"
                android:layout_marginBottom="8dp"
                android:background="@drawable/cirlce"
                android:fontFamily="@font/catamaran_bold"
                android:text="MAKE CALL"
                android:textColor="@color/colorPrimary"
                app:layout_constraintBottom_toTopOf="@+id/NumberNext"
                app:layout_constraintEnd_toEndOf="parent"
                app:layout_constraintStart_toEndOf="@+id/textView15"
                app:layout_constraintTop_toBottomOf="@+id/tableRow"
                app:layout_constraintVertical_bias="0.142" />

        </LinearLayout>

    </LinearLayout>


    <Button
        android:id="@+id/sendCodeButton"
        android:layout_width="351dp"
        android:layout_height="59dp"
        android:layout_marginBottom="28dp"
        android:background="@drawable/buttoncolor"
        android:fontFamily="@font/catamaran_bold"
        android:gravity="center"
        android:text="NEXT"
        android:textColor="@android:color/background_light"
        android:textSize="18sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent" />

    <Button
        android:id="@+id/phonedone"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_margin="28dp"
        android:background="@drawable/buttoncolor"
        android:fontFamily="@font/catamaran_bold"
        android:text="DONE"
        android:textColor="@android:color/background_light"
        android:textSize="18sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toTopOf="@+id/sendCodeButton"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        tools:visibility="gone" />

</androidx.constraintlayout.widget.ConstraintLayout>


```
