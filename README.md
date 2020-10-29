# OTRS-Customer-Portal-Additional-SignUp-Field
- Based on OTRS CE 6.0.30  
- Add another field at OTRS Customer Portal Registration / Sign-Up Page  


1) For example, UserMobile and UserCountry field (based on Config.pm / Defaults.pm),

		[ 'UserMobile',       Translatable('Mobile'),              'mobile',         1, 0, 'var', '', 0, undef, undef ],
		[ 'UserCountry',      Translatable('Country'),             'country',        1, 0, 'var', '', 0, undef, undef ],


2) For Mobile field, since this field is text input field,

	a) at Custom/Kernel/Output/HTML/Templates/Standard/CustomerLogin.tt,  
		inside Signup div

	***Input name and id must be same with the definition at Config mapping (e.g: Mobile)!!***  
	***You may want to build additional css class to check on validation value***  

		<!-- BEGIN ADDITIONAL FIELD -->
		<div class="NewLine">
			<label for="Mobile">[% Translate("Mobile Number") | html %]</label>
			<input title="[% Translate("Your mobile number") | html %]" type="text" name="Mobile"  id="Mobile" class="W50pc" value="[% Data.UserMobile | html %]"/>
		</div>
		<!-- END ADDITIONAL FIELD -->



3) For Country, I may want to make it as dropdown selection.

	a) So, defined it first in Custom/Kernel/Output/HTML/Layout.pm

	Just before this block,
	
		$Self->Block(
			Name => 'CreateAccountLink',
			Data => \%Param,
		);
	
	
	Add,
	
		#BEGIN ADDITIONAL FIELD
		my $CountryRef = [
			'Malaysia',
			'Singapore',
			'Indonesia',
			'Thailand',
		];
		
		##***Input name and id must be same with the definition at Config mapping (e.g: Country)!!***
		$Param{UserCountry} = $Self->BuildSelection(
			Data            => $CountryRef,       
			Name            => 'Country',        # name of element
			ID              => 'Country',         # (optional) the HTML ID for this element, if not provided, the name will be used as ID as well
			SelectedID  	=> '',
			Class           => 'Validate_Required Modernize W50pc', 
			PossibleNone   => 1, 
			Title          => 'Your Country',
			Multiple        => 0,                # (optional) default 0 (0|1)
			Size            => 1,                # (optional) default 1 element size
			TreeView       => 0,                 # (optional) default 0 (0|1)
		);
		#END ADDITIONAL FIELD 
	
	
	b) at Custom/Kernel/Output/HTML/Templates/Standard/CustomerLogin.tt, inside Signup div
	
		<!-- BEGIN ADDITIONAL FIELD -->
		<div class="NewLine">
			[% Data.UserCountry %]
        </div>
		<!-- END ADDITIONAL FIELD -->
		
		
4. (Optional) Manually update $OTRS_HOME/var/httpd/htdocs/skins/Customer/default/css/Core.Login.css 

	Increase height for example 400px for #Slider and #PreLogin. Play around with the css to suit your need.


 [![1.png](https://i.postimg.cc/FKQKdjX2/1.png)](https://postimg.cc/HrBmF8x2)  
 
 [![2.png](https://i.postimg.cc/JhczbG1t/2.png)](https://postimg.cc/rdsTTyRL)  
 
 [![3.png](https://i.postimg.cc/1tm5MstB/3.png)](https://postimg.cc/3yqTxM20)  
 
 [![4.png](https://i.postimg.cc/Twt24F56/4.png)](https://postimg.cc/sQZRBnJ6)  