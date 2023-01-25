pipeline {
  agent any
  stages {
	  

    stage ("Edit")
    {
      steps {
        
        script {
	 		       
	env.feature = input message: 'Please enter the feature you want to build with',
                             parameters: [string(defaultValue: '',
                                          description: '',
                                          name: 'Feature')]
        echo "Entered feature is "
	echo "${env.feature}"
		
        dir("/home/lakshmi/dell_pods/poky/build/conf") {
        sh '''#!/bin/bash
	
	echo "inside shell"
	#echo "${env.feature}"
	input=$(printenv feature)
	echo "your input"
	echo "$input"
       	
        #Locating the line with mentioned feature
        line=$(sed -n "/$input/p" local.conf | head -1)
        echo "$line"
	
	
        
        #extracting the line numbers 
        array=()
        
	n=$(grep -rin $input | head -1 | awk '{print $1 }' | cut -d: -f 2)
	echo "Line number is"
	echo "$n"
	
	lines=$(grep -rin $input | head -1 | awk '{print $1}' | cut -d# -f 2)
	echo "Number of lines to edit is"
	echo "$lines"
	
	
		
	        	             
        #Enabling the mentioned feature for build in local.conf 
	sum=$n
	for (( x=1 ; x<=$lines ; x++ )); 
	do
		echo "iterator is"	
	  	echo "$x"
		sum=$(($sum + 1))
		echo "$sum"
    		sed -i "$sum s/#//" local.conf
		
	done	 
	
	cd /home/lakshmi/dell_pods/poky
	pwd
	source oe-init-build-env
	pwd
	#rm -rf bitbake.lock
	
	
	#bitbake -c clean core-image-pods
	bitbake core-image-pods
	
	cd /home/lakshmi/dell_pods/poky/build/conf
	pwd
	echo "Restroing local.conf"
	
	#Disabling the feature after build is complete in local.conf
	sum1=$n
        for (( x=1 ; x<=$lines ; x++ )); 
	do
		echo "iterator is"	
	  	echo "$x"
		sum1=$(($sum1 + 1))
		echo "$sum"
    		sed -i "$sum1 s/^/#/" local.conf
		
	done	
        '''
	}  
    }
  }
   }
  }
}
Footer

