# Completed step definitions for basic features: AddMovie, ViewDetails, EditMovie 

Given /^I am on the RottenPotatoes home page$/ do
  visit movies_path
 end


 When /^I have added a movie with title "(.*?)" and rating "(.*?)"$/ do |title, rating|
  visit new_movie_path
  fill_in 'Title', :with => title
  select rating, :from => 'Rating'
  click_button 'Save Changes'
 end

 Then /^I should see a movie list entry with title "(.*?)" and rating "(.*?)"$/ do |title, rating| 
   result=false
   all("tr").each do |tr|
     if tr.has_content?(title) && tr.has_content?(rating)
       result = true
       break
     end
   end  
   assert result
 end

 When /^I have visited the Details about "(.*?)" page$/ do |title|
   visit movies_path
   click_on "More about #{title}"
 end

Then /^(?:|I )should see "([^"]*)"$/ do |text|
  if page.respond_to? :should
    page.should have_content(text)
  else
    assert page.has_content?(text)
  end
end

 When /^I have edited the movie "(.*?)" to change the rating to "(.*?)"$/ do |movie, rating|
  click_on "Edit"
  select rating, :from => 'Rating'
  click_button 'Update Movie Info'
 end


# New step definitions to be completed for HW3. 
# Note that you may need to add additional step definitions beyond these


# Add a declarative step here for populating the DB with movies.

Given /the following movies have been added to RottenPotatoes:/ do |movies_table|
  movies_table.hashes.each do |movie|

      Movie.create!(movie)
    
    # Each returned movie will be a hash representing one row of the movies_table
    # The keys will be the table headers and the values will be the row contents.
    # You should arrange to add that movie to the database here.
    # You can add the entries directly to the databasse with ActiveRecord methodsQ
  end
  
end

When /^I have opted to see movies rated: "(.*?)"$/ do |arg1|

  @all_ratings = Movie.all_ratings.split(', ').flatten #get all ratings in an array

  @keep_ratings = arg1.split(', ')#get all ratings to keep in an array

  @all_ratings.each do |rating| #iterate, unchecking all rating boxes
    page.uncheck("ratings[#{rating}]")
  end

  @keep_ratings.each do |rating| #iterate, checking boxes for ratings we want to keep
    page.check("ratings[#{rating}]")
  end

  click_button("Refresh")#refresh the page to reflect the changes


  # HINT: use String#split to split up the rating_list, then
  # iterate over the ratings and check/uncheck the ratings
  # using the appropriate Capybara command(s)


end

Then /^I should see only movies rated "(.*?)"$/ do |arg1|

  counter = 0 #keeps track of how many ratings matches we get
  should_have = 0 #we should have as many as adding up all find_all_by_ratings

  @keep_ratings.each do |rating| #iterate, calculating the value we should end up with
    should_have = should_have + Movie.find_all_by_rating(rating).size
  end


  all("td").each do |td| #get all td from the page

    if @all_ratings.include?(td.text) #if the text of the td is a rating
      counter = counter + 1 #increment rating counter
    end

  end

  assert !!(counter == should_have) #if the number of td ratings equals the expected ratings, then pass. otherwise fail.

end

Then /^I should see all of the movies$/ do
#essentially the same as above. I know this violates "DRY"ness, but I couldn't get this to work otherwise!

  counter = 0

  all("td").each do |td|
    
    if @all_ratings.include?(td.text)
      counter = counter + 1
    end

  end

  assert !!(counter == Movie.all.size)#we should have found as many movies as are in the whole movie table

end



When(/^I click 'Movie Title'$/) do
  visit movies_path(:sort => 'title', :ratings => @all_ratings) #sort page by title, include all ratings
end

Then(/^I should see "(.*?)" before "(.*?)"$/) do |arg1, arg2|

  counter = 0 #keep a counter

  found_arg1 = false #booleans to flag if we found arg1
  found_arg2_after_arg1 = false #flag if we found arg2 in the list AFTER arg1

  all("td").each do |td| #get all td
    
    if(counter % 4 == 0) #use modulus to check every 4th td, starting with the first one. All tds that don't contain movie titles are discarded
      if(td.text == arg1) #if the current td is arg1, then arg1 is found
        found_arg1 = true
      end

      if(td.text == arg2) #if the current td is arg2...

        if(found_arg1 == false) #if we haven't found arg1 yet, fail!
          break
        else #else, arg2 was found AFTER arg1, pass!
          found_arg2_after_arg1 = true
          break
        end    

      end

    end

    counter = counter + 1 #increment counter after each td is processed
  end

  assert !!(found_arg2_after_arg1) #return true if arg2 was found after arg1, false otherwise

end

When(/^I click 'Release Date'$/) do
  visit movies_path(:sort => 'release_date', :ratings => @all_ratings) #sort movies by release date, include all ratings
end






