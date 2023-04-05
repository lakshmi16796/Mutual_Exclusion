pipeline {
  agent any
	
environment{
				
	USER_NAME = "PODS"
	USER_PWD = "pods"
}
  stages 
{  
   
    stage ("User Input Stage")
    {
	    
      steps {	 
	      
	script {
		
                 echo "selected parameter is"		      
	         echo "${env.choices}"	
		 echo "${env.adv_choices}"
		
		 //sh "printenv | sort"
		 if(env.choices == "Xen,QT")
		{
			echo "Xen and QT cannot be selected together"
			
			def mutualExChoice = input(
                            id: 'ExChoice', message: 'Please select either Xen or QT',
                            parameters: [

                                    string(defaultValue: '',
                                            description: 'Mutual Exclusion choice',
                                            name: 'ExChoice'),
                            
                            ])
			echo "Selected choice is: ${mutualExChoice}"
			env.choices = "${mutualExChoice}"
		}
		
		 if(env.adv_choices == "Docker")
		{
			echo "Advanced feature selected"
			//env.getEnvironment().each { name, value -> println "Name: $name -> Value $value" }
			//input message: 'enter password', parameters: [password(defaultValue: 'value', description: '', name: 'hidden')]
			//echo "${env.MYVARNAME_USR}"
			
			def userCred = input(
                            id: 'userCred', message: 'Enter the Credentials to access advanced feature',
                            parameters: [

                                    string(defaultValue: '',
                                            description: 'The Build Password',
                                            name: 'BuildPwd'),
                            
                            ])
			echo "User credential is: ${userCred}"
			
			if( "${userCred}" == "${env.USER_PWD}")
			{
                		echo "Matched"
            		} 
			else
			{
              			 echo "Not matched"
				 return 
           		}
		
			
		}
	    
		dir("/home/lakshmi/dell_pods/poky/build/conf")  
		  {
      	        	sh '''#!/bin/bash
			input=$(printenv choices)
			input_adv=$(printenv adv_choices)
			input+="$(echo " ") $input_adv "
			echo "your input"
			echo "$input"
			
			#Selecting common features and storing it into Properties file
			n=$(grep -rin "#CO#" local.conf | awk '{print $2 }')
			echo "Common pattern is"
			#echo $n
			x=$(echo $n | sed 's/ /,/g')
			#echo "x is"
			echo $x
			> Properties
			echo "feature=$x" >> Properties
			
			#Selecting Advanced features and storing it into adv_properties file
			n=$(grep -rin "#AD#" local.conf | awk '{print $2 }')
			echo "Advanced pattern is"
			#echo $n
			y=$(echo $n | sed 's/ /,/g')
			#echo "x is"
			echo $y
			> adv_properties
			echo "adv_feature=$y" >> adv_properties
			
			array=()
			IFS='+, ' read -ra array <<< $input
			for i in "${array[@]}"; do
  				echo "$i"
			
			#line=$(sed -n "/$i/p" local.conf | head -1)
        		#echo "$line"
		
			n=$(grep -rin $i local.conf | head -1 | awk '{print $1 }' | cut -d: -f 1)
			echo "Line number is"
			echo "$n"
		
			lines=$(grep -rin $i local.conf | head -1 | awk '{print $1}' | cut -d# -f 3)
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
			done
			
			
	
	
			cd /home/lakshmi/dell_pods/poky/build/conf
			pwd
			
			echo "Restroing local.conf"
	
		     for i in "${array[@]}"; do
  			echo "$i"
		
			#line1=$(sed -n "/$i/p" local.conf | head -1)
        		#echo "$line1"
		
			n1=$(grep -rin $i local.conf | head -1 | awk '{print $1 }' | cut -d: -f 1)
			echo "Line number is"
			echo "$n1"
			
			lines1=$(grep -rin $i local.conf | head -1 | awk '{print $1}' | cut -d# -f 3)
			echo "Number of lines to edit is"
			echo "$lines1"
		
			#Disabling the mentioned feature for build in local.conf 
			sum1=$n1
			for (( x=1 ; x<=$lines1 ; x++ )); 
			do
				echo "iterator is"	
	  			echo "$x"
				sum1=$(($sum1 + 1))
				echo "$sum1"
    				sed -i "$sum1 s/^/#/" local.conf
			done
		done
			
					
			'''
		  }
		
		
		} 
  }
  }
  }
  
  }

