---
layout: post
title: Vier op 'n rij
description: Vier op 'n rij spel
published: true
img: /img/connect4.jpg
---
<h1 class="center">
<a href="https://github.com/michaelarayabe/connect_four" target="_blank">Github Repo</a> 
</h1>
<br>
<h1>Wat is dit?</h1>
<br>

<p>Een command-line Vier op 'n rij-spel dat ik schreef tijdens het oefenen via  <a href="http://www.theodinproject.com" target="none">The Odin Project</a>.</p>

<br>
<h1>welke soort probleem lost het op?</h1>
<br>
<p>
<a href="https://nl.wikipedia.org/wiki/Vier_op_%27n_rij" target="none"> Vier op 'n rij</a> is een spel voor twee spelers met als doel als eerste speler een aaneengesloten rij van vier schijven te vormen. Het spel wordt gespeeld op een verticaal geplaatst bord bestaand uit 7 kolommen en 6 rijen. Iedere speler beschikt over 21 schijven met zijn eigen kleur, meestal geel en rood. De spelers laten om de beurt een schijf in een van de nog niet volle kolommen vallen. De schijf bezet altijd het laagst beschikbare vak in een kolom. Een speler wint door met vier of meer van zijn schijven een aaneengesloten rij te vormen. Vaak roept de speler hierbij luid en duidelijk "Vier op een rij!" Een rij kan zowel verticaal, horizontaal als diagonaal worden gevormd en beëindigt het spel. Het spel eindigt in een gelijkspel als geen van de twee spelers erin slaagt een aaneengesloten rij te vormen voordat het bord volledig door schijven is gevuld.</p>

<br>
<h1>Wat zit erin?</h1>
<br>

<p>
Een Ruby-script dat ik heb geoefned om complexe problemen op te delen in kleine  onderdelen, en die onderdelen vervolgens te testen om ervoor te zorgen dat mijn code kan worden aangepast als ik dit begrijp en meer te weten kom.

<br>
Eerst en vooral, door dit spel te schrijven, was het mogelijk voor mij om OOP- en TDD-principes te begrijpen. Mijn bestandsstructuur ziet er zo uit:</p>

{% highlight ruby %}
  .
  ├── connect_four.rb
  ├── lib
  │   ├── board.rb
  │   ├── game.rb
  │   └── player.rb
  └── spec
      ├── board_spec.rb
      ├── game_spec.rb
      ├── player_spec.rb
      └── spec_helper.rb
{% endhighlight %}

<p>Elke class was onderdeel van zijn eigen bestand en elke class had zijn eigen test. Het andere leukste deel van het bouwen van dit spel was het schrijven van de methode <code> Board # win? </code>. Ik benaderde deze uitdaging een paar verschillende manieren. Het eerste ding te merken is dat ik een nested array gebruikte om mijn bord te vertegenwoordigen:</p>

{% highlight ruby %}
[['.', '.', '.', '.', '.', '.', '.'],
 ['.', '.', '.', '.', '.', '.', '.'],
 ['.', '.', '.', '.', '.', '.', '.'],
 ['.', '.', '.', '.', '.', '.', '.'],
 ['.', '.', '.', '.', '.', '.', '.'],
 ['O', '.', '.', '.', '.', '.', '.']]
{% endhighlight %}


<p>Dus daarom kan de positie waar de <code> 'O' </code> zich bevindt worden geïndexeerd als <code> bord [5] [0] </code> (Het eerste element in de vijfde array). Toen het tijd werd om een <code> Board # win </code> methode te schrijven, eerst benaderde ik het met recursief. Ik wilde dat de methode true of false returns door een "flood-fill" -methode te volgen. Het idee was dat wanneer een token werd gespeeld, die "coördinaten" zouden worden doorgegeven aan <code> Board # win? </code>. De basishulzen zouden zijn om <code> terug te zenden if count == 4 </code>, wat betekent dat vier tokens in volgorde werden gevonden,en om te <code>  if! row.between? (0, 5) &&! column.bettween? (0, 6) </code> return, wat betekent dat de methode nu buiten het bereik van het bord aan het testen was.<br><br>Als de huidige positie op het bord gelijk was aan het token,  zou de methode elke positie in elke richting controleren om te zien of er een overeenkomende token was, als dat zo is, zou het in die richting gaan en <code> count + = 1 </code> totdat <code> count == 4 </code> of een overeenkomende token is niet gevonden.In theorie moet dit werken, en ik ben nog steeds enthousiast over het vooruitzicht van de implementatie van dit algoritme, omdat ik denk dat het geschikt is, maar in plaats van heb ik genoegen genomen met een non-recursive strategie:</p>

{% highlight ruby %}
def horizontal_win?(args = {})
  token   = args.fetch(:token, nil)
  row     = args.fetch(:row, nil)
  column  = args.fetch(:column, nil)

  count = 0

  4.times do |i|
    count += 1 if column + i <= 6 && @play_area[row][column + i] == token
    count += 1 if column - i >= 0 && @play_area[row][column - i] == token
    return true if count == 5
  end

  false
end
{% endhighlight %}

<p>Deze methode is geschreven om te controleren de horizontale, diagonale en verticale wins. 

{% highlight ruby %}
  it 'returns false' do
    board.instance_variable_set(:@play_area,
                                [['.', '.', '.', '.', '.', '.', '.'],
                                 ['.', '.', '.', '.', '.', '.', '.'],
                                 ['.', '.', '.', '.', '.', '.', '.'],
                                 ['.', '.', '.', '.', '.', '.', '.'],
                                 ['.', '.', '.', '.', '.', '.', '.'],
                                 ['O', '.', '.', '.', 'O', 'O', 'O']])
    expect(board.win?(token: 'O', row: 4, column: 0)).to be false
  end
{% endhighlight %}

<p>Deze test is mislukt. <br> <br> Over het algemeen was dit een van mijn favoriete projecten om aan te werken. Van begin tot eind voelde ik me zelfverzekerd en nu ben ik ben enthousiast om terug te gaan en te refractor! </p>

<br>

<h1>Wat heb ik geleerd?</h1>
<br>
<p>Ik heb geleerd TDD met RSpec, OOP en separation of concerns!</p> 