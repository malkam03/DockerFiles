FROM imsky/haxe:latest
MAINTAINER Malcolm Davis <me@malcolmdavis.xyz>
# JAVA SDK
RUN add-apt-repository ppa:webupd8team/java -y && apt-get update
RUN echo "oracle-java8-installer shared/accepted-oracle-license-v1-1 select true" | debconf-set-selections && apt-get install -y oracle-java8-installer && apt-get install -y oracle-java8-set-default
# Dependencies
RUN apt-get install sudo
RUN haxelib install nme 5.7.1 && haxelib install actuate 1.8.7 && haxelib install lime 5.4.0 && haxelib run nme setup && haxelib run lime setup -y && apt-get install unzip -y
######## Android Tools ########
# Android SDK
RUN wget http://dl.google.com/android/android-sdk_r24.2-linux.tgz && tar -xvf android-sdk_r24.2-linux.tgz -C ~ && rm -f android-sdk_r24.2-linux.tgz &&  echo "export ANDROID_HOME=~/android-sdk-linux/" >> ~/.bashrc
RUN cd ~/android-sdk-linux/tools && (while sleep 3; do echo "y"; done) |  ./android update sdk -u -a -t 1,2,4,29,173 && echo "export PATH=\${PATH}:~/android-sdk-linux/tools" >> ~/.bashrc && echo "export PATH=\${PATH}:~/android-sdk-linux/platform-tools" >> ~/.bashrc && source ~/.bashrc
#Android NDK
RUN cd ~ && wget https://dl.google.com/android/repository/android-ndk-r14b-linux-x86_64.zip && unzip android-ndk-r14b-linux-x86_64.zip -d /opt && rm -f android-ndk-r14b-linux-x86_64.zip && echo "export PATH=\${PATH}:/opt/android-ndk-r14b" >> ~/.bashrc
# Apache ANT
#RUN apt-get install ant -y
RUN wget https://archive.apache.org/dist/ant/binaries/apache-ant-1.9.2-bin.tar.gz && tar -xvf apache-ant-1.9.2-bin.tar.gz -C /usr/local && rm -f apache-ant-1.9.2-bin.tar.gz && printf "#ANT PATH\nexport ANT_HOME=/usr/local/ant\nexport PATH=\${ANT_HOME}/bin:\${PATH}" >>  ~/.bashrc && source  ~/.bashrc
##############################
# Setup enviroment Variables
RUN printf "# JAVA Path\nexport JAVA_HOME=/usr/lib/jvm/java-8-oracle/\n# ANDROID NDK Path\nexport ANDROID_NDK_HOME=/opt/android-ndk-r14b" >> ~/.bashrc && source ~/.bashrc
# Android setup
RUN printf  "n\nn\nn\n~/android-sdk-linux/\n/opt/android-ndk-r14b\n/usr/lib/jvm/java-8-oracle/\n" | lime setup android
RUN printf "<xml>\n<section id=\"vars\">\n		<set name=\"ANDROID_SETUP\" value=\"true\" />\n		<set name=\"ANDROID_NDK_ROOT\" value=\"/opt/android-ndk-r14b/\" />\n		<set name=\"ANDROID_SDK\" value=\"/root/android-sdk-linux/\" />\n		<set name=\"ANT_HOME\" value=\"/usr/local/apache-ant-1.9.2/\" />\n		<set name=\"JAVA_HOME\" value=\"/usr/lib/jvm/java-8-oracle/jre/bin/java\" />\n	</section>\n</xml>" >  ~/.hxcpp_config.xml
# NME
RUN haxelib remove hxcpp && haxelib install hxcpp 3.4.64 && echo "y" | haxelib set hxcpp 3.4.64 && apt-get install -y lib32z1 lib32ncurses5 lib32stdc++6
# Cleaning
RUN apt-get clean && apt-get autoremove &&  haxelib remove lime-samples
# Create and go to workspace
RUN mkdir /tmp/haxe
WORKDIR /tmp/haxe
