---
layout: post
title: android-singleton
date: '2012-04-12'
cateory: android
tags: [android,singleton]
---

##说明
>    在android项目中，我们经常会需要用到一些singleton模式，除了java中常用的单例模式的写法，这里提出一种非静态的单例模式，而且还带有android的生命周期，生命周期跟随application而变化

* 创建一个这样的单例类(是不是和java的单利模式很像)  

        public class Singleton {
            private Singleton(Application application) {
                this.application = application;
            }
        
            public static Singleton getInstance(Context context) {
                Application app = (Application) context.getApplicationContext();
                if (null == app.getSingleton()) {
                    app.setSingleton(new Singleton(app));
                }
                return app.getSingleton();
            }
        
            public void onTerminate() {
            }
        
            public void onLowMemory() {
            }
        
            public void onTrimMemory(int level) {
            }
        
            //other methods
        
        }
    
* 在application 中这样写

        public class MyApplication extends Application {
            private Singleton singleton;
        
            @Override
            public void onCreate() {
                super.onCreate();
                singleton=Singleton.getInstance(this);
            }
        
            public Singleton getSingleton() {
                return singleton;
            }
        
            public void setSingleton(Singleton singleton) {
                this.singleton = singleton;
            }
        
            @Override
            public void onTerminate() {
                super.onTerminate();
                singleton.onTerminate();
            }
        
            @Override
            public void onConfigurationChanged(Configuration newConfig) {
                super.onConfigurationChanged(newConfig);
            }
        
            @Override
            public void onLowMemory() {
                super.onLowMemory();
                singleton.onLowMemory();
            }
        
            @Override
            public void onTrimMemory(int level) {
                super.onTrimMemory(level);
                singleton.onTrimMemory(level);
            }
        }
    
* 配置mainfest

          <application
                android:name=".MyApplication"
            
* 使用，是不是很简单，但是是单例的，因为application只有一个  

         Singleton.getInstance(this)
