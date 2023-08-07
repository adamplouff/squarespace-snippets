<script>
  import { onMount } from "svelte";

  const snippet = () => {
      var permalinkElements = Array.from(document.querySelectorAll('h2, h4'));

      var tocElement = document.querySelector('toc');

      var tocArray = []
      var tableOfContents = document.createElement('div');
      tableOfContents.setAttribute('class', 'toc');

      // Iterate through each element
      for (var i = 0; i < permalinkElements.length; i++) {
        var element = permalinkElements[i];
        // Get the text within the element
        var text = element.innerText.trim();

        // Generate an ID based on the element text
        var id = text.toLowerCase().replace(/\s+/g, '-').replace(/[^a-zA-Z0-9\-]/g, '');

        if (element.tagName === 'H2') {
          var newElement = document.createElement('h2');
          newElement.setAttribute('id', id);
          newElement.appendChild(document.createTextNode(text));
          element.parentNode.replaceChild(newElement, element);

          tocArray.push({
            text: element.innerText.trim(),
            id : id,
            children: []
          });


        } else if (element.tagName === 'H4') {

          var newElement = (element.tagName === 'H3') ? document.createElement('h3') : (element.tagName === "H4") ? document.createElement('h4') : null;

          // Set the ID attribute
          newElement.setAttribute('id', id);

          // Create the anchor link
          var anchorLink = document.createElement('a');
          anchorLink.setAttribute('href', '#' + id);
          anchorLink.setAttribute('class', 'permalink');
          anchorLink.innerHTML = '#';

          // Append the anchor link to the new element
          newElement.appendChild(anchorLink);

          // Create a text node with the element text
          var textNode = document.createTextNode(text);

          // Append the text node to the new element
          newElement.appendChild(textNode);

          // Replace the element with the new element
          element.parentNode.replaceChild(newElement, element);
        
          tocArray[tocArray.length - 1].children.push({
            text: text,
            id: id
          });
        }
      }

      console.log('tocArray', tocArray);

      // convert tocArray to an unordered list of links
      tocArray.forEach(function (element) {
        var section = document.createElement('div');
        var headingLink = document.createElement('a');
        headingLink.setAttribute('href', '#' + element.id);
        var heading = document.createElement('h3');
        var h2Text = element.text;
        heading.textContent = h2Text;
        headingLink.appendChild(heading);
        section.appendChild(headingLink);
        

        var subList = document.createElement('ul');
        element.children.forEach(function (child) {
          var subHeading = document.createElement('li');
          var anchorLink = document.createElement('a');
          anchorLink.setAttribute('href', '#' + child.id);
          console.log(child.text);
          anchorLink.innerHTML = child.text;
          subHeading.appendChild(anchorLink);
          subList.appendChild(subHeading);
        });
        section.appendChild(subList);
        tableOfContents.appendChild(section);
      });
      tocElement.parentNode.replaceChild(tableOfContents, tocElement);
  }

  onMount(() => {
    snippet();
  });
</script>

<svelte:head>
	<title>Help + battleaxe.co</title>
</svelte:head>


<main>
  <div>
    TOC
    <toc></toc>
  </div>

  <hr>

  <div>
    <h2>Purchasing</h2>
    <h4> When do I get my download? </h4>
    <h3> Immediately </h3>
    <p> As soon as you purchase you will receive an email from @paddle.com with a confirmation receipt, then a second email from @battleaxe.co with the license key and download info. </p>
    <p> If you do not receive both of these emails, check the spam folder. </p>
    <p> If they aren’t in the spam folder, the email address could have been misspelled at checkout. Send us a message and we will look into the issue. </p>

  </div>

  <hr>

  <div>
    <h1>Snippet</h1>

    { snippet }
  </div>
  
</main>