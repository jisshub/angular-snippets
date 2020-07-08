
common error on template
---------------------------
<form name="covidForm">
	<div class="form-group">
	  <label for="country">Country</label>
	 
	  <input type="text" class="form-control" name="countryName"
	  [(ngModel)]="countryName">
</div>

 <!-- always use name attribute for all inputs awhile using forms else angular throws errors-->
 
 
 Can't bind to 'ngModel' since it isn't a known property of 'select
 
 solution: <https://stackoverflow.com/questions/43298011/angular-error-cant-bind-to-ngmodel-since-it-isnt-a-known-property-of-inpu>
 
 
 
