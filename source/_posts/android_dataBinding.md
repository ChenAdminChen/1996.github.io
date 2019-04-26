---
title: android_DataBinding
date: 2019.01.16 16:35:34
tags:
  - android
  - DataBinding
---

## DataBinding

&nbsp;&nbsp;&nbsp;&nbsp;DataBinding用于Fragment与UI交互数据
&nbsp;&nbsp;&nbsp;&nbsp;做法如下：

>build.gradle

```
android{
   ...
 dataBinding {
        enabled = true
    }
    ...
}
```

>.xml
```
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:bind="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>

        <import type="com.yf.axworker.model.TrainingHistory" />

        <import type="android.bluetooth.BluetoothGatt" />
        
         <import type="android.databinding.ObservableField" />
       
         <variable
             name="conclusionH"
             type="ObservableField&lt;String&gt;" />
        
        <variable
            name="bleStatus"
            type="android.databinding.ObservableInt" />

        <variable
            name="angle"
            type="android.databinding.ObservableByte" />

        <variable
            name="healthData"
            type="com.yf.axworker.data.model.HealthData" />

    </data>

    <android.support.constraint.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context="com.yf.axworker.ui.fragment.TrainingFragment">
        
        <TextView
            android:id="@+id/lbl_cancel_training"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="8dp"
            android:text="@{bleStatus}"
            app:layout_constraintEnd_toEndOf="@+id/btn_finish_training"
            app:layout_constraintStart_toStartOf="@+id/btn_finish_training"
            app:layout_constraintTop_toBottomOf="@+id/btn_finish_training" />

        <TextView
            android:id="@+id/txt_cancel_training"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="8dp"
            android:text="@{healthData.id}"
            app:layout_constraintEnd_toEndOf="@+id/btn_finish_training"
            app:layout_constraintStart_toStartOf="@+id/btn_finish_training"
            app:layout_constraintTop_toBottomOf="@+id/btn_finish_training" />
       
    </android.support.constraint.ConstraintLayout>
</layout>

```

.java

```java

package com.yf.axworker.ui.fragment;

import android.animation.Animator;
import android.animation.AnimatorInflater;
import android.bluetooth.BluetoothGatt;
import android.content.Context;
import android.databinding.DataBindingUtil;
import android.databinding.Observable;
import android.databinding.ObservableByte;
import android.databinding.ObservableInt;
import android.os.Bundle;
import android.os.Handler;
import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.support.design.widget.TabLayout;
import android.support.v4.app.Fragment;
import android.support.v7.app.ActionBar;
import android.view.GestureDetector;
import android.view.LayoutInflater;
import android.view.MotionEvent;
import android.view.View;
import android.view.ViewGroup;

import com.yf.axworker.R;
import com.yf.axworker.data.model.HealthData;
import com.yf.axworker.databinding.FragmentTrainingBinding;
import com.yf.axworker.event.BleConnectionReport;
import com.yf.axworker.event.BleServicesDiscoveredEvent;
import com.yf.axworker.event.HrReport;
import com.yf.axworker.event.HrReportEvent;
import com.yf.axworker.event.PersonalHrReportEvent;
import com.yf.axworker.model.TrainingHistory;
import com.yf.axworker.model.User;
import com.yf.axworker.ui.adapter.SafetyAidChartsAdapter;
import com.yf.axworker.ui.base.BaseFragment;
import com.yf.axworker.utils.Constants;
import com.yf.axworker.utils.UserInfoUtil;

import org.greenrobot.eventbus.EventBus;
import org.greenrobot.eventbus.Subscribe;
import org.greenrobot.eventbus.ThreadMode;

import java.util.ArrayList;
import java.util.Date;
import java.util.HashMap;
import java.util.List;
import java.util.concurrent.TimeUnit;

import lombok.extern.slf4j.Slf4j;

@Slf4j
public class TrainingFragment extends BaseFragment {

    FragmentTrainingBinding binding;

    private Animator aniBleRotation;

    private ObservableInt bleStatus = new ObservableInt(BluetoothGatt.STATE_CONNECTED);
    private ObservableByte angle = new ObservableByte();
    private ObservableInt duration = new ObservableInt();

    private ObservableField<String> conclusionH = new ObservableField<>();

    TrainingHistory trainingInfo;

    //
    public TrainingFragment() {
        // Required empty public constructor
    }


    @Override
    public void onAttach(Context context) {
        super.onAttach(context);

        if (context instanceof FragmentAction) {
            mListener = (FragmentAction) context;
        } else {
            throw new RuntimeException(context.toString()
                    + " must implement FragmentAction");
        }

        baseActivity.getActivityComponent().inject(this);

    }

    @Override
    public void onDetach() {
        super.onDetach();
        mListener = null;
    }

    @Override
    public void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

    }

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        binding = DataBindingUtil.inflate(inflater, R.layout.fragment_training, container, false);

        return binding.getRoot();
    }

    @Override
    public void init() {

        tryGetTrainingAngle();

        //
        binding.setController(controller);

        //綁定数据
        binding.setBleStatus(bleStatus);
        binding.setTrainingInfo(trainingInfo);
        binding.setDuration(duration);
        binding.setConclusionH(conclusionH);
        test();
    }
    
    //change duration value
    public void test(){
        duration.set(4);
    }
}

```
    
  